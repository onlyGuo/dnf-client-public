<template>
    <div class="title-box">
        <div class="title">
            <div style="
                            height: 0;
                            width: 0;
                            border-left: transparent solid 15px;
                            border-bottom: transparent solid 15px;
                            border-top: #1f2b38 solid 15px;
                            border-right: #1f2b38 solid 15px;
                            display: inline-block;
                            vertical-align: top;
                            margin-right: -5px;
                           "></div>
            <div style="height: 30px;
                        line-height: 30px;
                        display: inline-block;
                        padding: 0 10px;
                        background-color: #1f2b38;
                        font-size: 13px;
                        "
            v-text="text"></div>

            <div style="
                            height: 0;
                            width: 0;
                            border-bottom: transparent solid 15px;
                            border-right: transparent solid 15px;
                            border-top: #1f2b38 solid 15px;
                            border-left: #1f2b38 solid 15px;
                            display: inline-block;
                            vertical-align: top;
                            margin-left: -5px;
                           "></div>
        </div>
    </div>
</template>

<script>
export default {
    name: "TitleBar",
    props: {
        text: String
    }
}
</script>

<style scoped>
.title{
    height: 10px;
    background-color: #1f2b38;
}
.title-box{
    height: 30px;
    -webkit-app-region: drag;
    -webkit-user-select: none;
}
</style>