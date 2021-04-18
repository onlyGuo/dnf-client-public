<template>
    <div class="box" v-if="show">
        <div class="loader"></div>
        <div class="message" v-text="message"></div>
    </div>
</template>

<script>
const fs = require('fs');
const request = require('request');

export default {
    name: "UpdateBox",
    data(){
        return {
            message: '正在检查更新...',
            show: true,
            successCount: 0
        }
    },
    mounted() {
        this.doUpload();
    },
    methods: {
        downloadFile(file_url , targetPath, progressFun, finishFun){
            // Save variable to know progress
            let received_bytes = 0;
            let total_bytes = 0;

            let req = request({
                method: 'GET', uri: file_url
            });

            let out = fs.createWriteStream(targetPath);
            req.pipe(out);

            req.on('response', function ( data ) {
                // Change the total bytes value to get progress later.
                total_bytes = parseInt(data.headers['content-length' ]);
            });

            req.on('data', function(chunk) {
                // Update the received bytes
                received_bytes += chunk.length;
                progressFun({
                    received: received_bytes,
                    total: total_bytes
                });
            });

            req.on('end', function() {
                finishFun("File succesfully downloaded");
            });
        },
        progressFun(type){
            this.message = "正在更新第" + (parseInt(this.successCount) + 1) + "个文件(" + parseInt(type.received / type.total * 100) + '%)';
        },

        doUpload(){
            let home = this.$electron.remote.app.getPath('home')
            console.log(home)
            fs.readFile('../version.json', 'utf-8', (err, data) => {
                let version = {};
                if (err){
                    version = {
                        "Script.pvf": "1",
                        "DNF.exe": "1",
                        "DNF.toml": "1"
                    }
                    fs.writeFileSync('../version.json', JSON.stringify(version), 'utf-8')
                }else{
                    version =JSON.parse(data);
                }

                this.$http.get(this.BASE_HTTP + "/version.json").then(res => {
                    let uplodates = [];
                    for (let i in res.data){
                        if (!version[i] || res.data[i] !== version[i]){
                            uplodates.push({
                                key: i,
                                path: res.data[i]
                            });
                        }
                    }
                    if (uplodates.length === 0){
                        this.show = false;
                        return;
                    }

                    // 逐个更新
                    for (let i in uplodates){
                        this.downloadFile( uplodates[i].path, '../' + uplodates[i].key, this.progressFun, () => {
                            this.successCount ++;
                            if (this.successCount === uplodates.length){
                                // 更新版本配置
                                fs.writeFileSync('../version.json', JSON.stringify(res.data), 'utf-8')
                                this.show = false;
                            }
                        });
                    }

                }).catch((e) => {
                    alert('连接服务器失败');
                    console.log(e)
                    this.$electron.remote.app.quit();
                })

            })
        }
    }
}
</script>

<style scoped>
    .message{
        position: fixed;
        bottom: 250px;
        width: 100%;
        text-align: center;
    }
    .box{
        background-color: rgba(0, 0, 0, .8);
        position: fixed;
        left: 0;
        top: 180px;
        right: 0;
        bottom: 210px;
        z-index: 1;
        backdrop-filter: blur(15px);
        -webkit-user-select: none;
    }
    .loader,
    .loader:before,
    .loader:after {
        background: #42b983;
        animation: load1 1s infinite ease-in-out;
        width: 1em;
        height: 4em;
    }
    .loader:before,
    .loader:after {
        position: absolute;
        top: 0;
        content: '';
    }
    .loader:before {
        left: -1.5em;
    }
    .loader {
        text-indent: -9999em;
        margin: 80px auto;
        position: relative;
        font-size: 11px;
        /* 延时0.16s */
        animation-delay: 0.16s;
    }
    .loader:after {
        left: 1.5em;
        /* 延时0.32s */
        animation-delay: 0.32s;
    }
    @keyframes load1 {
        0%,
        80%,
        100% {
            box-shadow: 0 0 #42b983;
            height: 4em;
        }
        40% {
            /* 实现上部拉伸 */
            box-shadow: 0 -2em #42b983;
            /* 实现下部拉伸 */
            height: 5em;
        }
    }
</style>