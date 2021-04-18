<template>
    <div class="home">
        <div class="logo"/>
        <div class="from">
            <div class="label">
                <label>
                    <span>账号:</span>
                    <input type="text" v-model="form.accountname"/>
                </label>
            </div>
            <div class="label">
                <label>
                    <span>密码:</span>
                    <input type="password" v-model="form.password"/>
                </label>
            </div>

            <a href="javascript:;" class="reg-btn" @click="$electron.remote.shell.openItem(regedit)">注册账号</a>
            <button @click="login" v-text="loginMessage"></button>
            <button class="logout" @click="$electron.remote.app.quit()">退出</button>
        </div>

        <div style="-webkit-user-select: none;">
            <div class="line"></div>
            <div style="background-color: #2c3e50; display: inline-block; margin-top: -10px; margin-left: -25px; position: fixed; width: 50px">
                公告
            </div>
        </div>
        <div>
            <pre style="text-align: left; padding: 10px;" v-text="notice">正在获取...</pre>

        </div>

        <!-- 更新器 -->
        <update-box></update-box>

        <message-box :option="messageOption"></message-box>
    </div>
</template>

<script>

import UpdateBox from "../components/UpdateBox";
import MessageBox from "../components/MessageBox";

const exec = require('child_process').exec
export default {
    name: 'Home',
    components: {MessageBox, UpdateBox},
    data(){
        return {
            form: {
                accountname: '',
                password: ''
            },
            notice: '',
            regedit: this.BASE_HTTP + '/register',
            messageOption: {
                message: '账号不能为空',
                show: false
            },
            loginMessage: '登录'
        }
    },
    methods: {
        login(){
            if (this.loginMessage !== '登录'){
                return;
            }
            if (!this.form.accountname){
                this.messageOption.message = '账号不能为空';
                this.messageOption.show = true;
                return;
            }
            if (!this.form.password){
                this.messageOption.message = '密码不能为空';
                this.messageOption.show = true;
                return;
            }
            this.loginMessage = '登录中...';
            this.$http.post(this.BASE_HTTP + "/api/v1/client/login", this.form).then(res => {
                console.log(res.data);
                exec('cd ../ & dnf.exe ' + res.data)
                //this.$electron.remote.shell.openItem('dnf.exe ' + res.data);
                setTimeout(() => {
                    this.$electron.remote.app.quit();
                }, 1000)
            }).catch(e => {
                if (e.response.status === 400){
                    this.messageOption.message = e.response.data.message;
                    this.messageOption.show = true;
                }else{
                    this.messageOption.message = '网络错误，请稍后再试';
                    this.messageOption.show = true;
                }
                this.loginMessage = '登录'
            }).finally(() => {
                this.loginMessage = '登录'
            })
        }
    },
    mounted() {
        this.$http.get(this.BASE_HTTP + '/api/v1/notice').then(res => {
            this.notice = res.data.content;
        })
    }
}
</script>

<style scoped>
.logo {
    background: url("../assets/dnf.png") no-repeat;
    background-size: cover;
    height: 100px;
    width: 150px;
    margin: 20px auto;
    -webkit-app-region: drag;
    -webkit-user-select: none;
}
.from{
    min-height: 20px;
    padding: 20px;
    text-align: left;
}
.from .label span{
    margin: 20px 15px 20px 20px;
    display: inline-block;
    vertical-align: top;
    -webkit-user-select: none;
}
.from .label input{
    vertical-align: top;
    margin-top: 18px;
    outline: none;
    background-color: #233040;
    border: solid #3c536a 1px;
    height: 26px;
    line-height: 26px;
    color: white;
    padding: 0 5px;
    width: 200px;
}

.from .label input:hover{
    border: solid #48637f 1px;
    background-color: #1f2b38;
}

.from .label input:focus{
    border: solid #48637f 1px;
    background-color: #1f2b38;
}

.from .reg-btn{
    color: #42b983;
    font-size: 14px;
    display: block;
    text-align: right;
    margin-right: 20px;
}

.from button{
    background-color: #1f2b38;
    border: #48637f solid 1px;
    color: #c9c9c9;
    padding: 5px;
    width: 200px;
    display: block;
    margin: 20px auto;
    outline: none;
}

.from button:hover{
    background-color: #233040;
}

.from .logout{
    background-color: red;
    border: #1f2b38 solid 1px;
}

.from button.logout:hover{
    background-color: #e50000;
}

.line{
    border: #42b983 solid 1px;
}
</style>
