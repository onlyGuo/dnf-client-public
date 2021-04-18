<template>
    <div class="bg" v-if="option.show">
        <div class="box">
            <div v-text="option.message"></div>
            <button @click="option.show = false">确定</button>
        </div>
    </div>
</template>

<script>
export default {
    name: "MessageBox",
    props: {
        option: Object
    }
}
</script>

<style scoped>
    .bg{
        background-color: rgba(255, 255, 255, .4);
        position: fixed;
        z-index: 2;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        -webkit-user-select: none;
    }
    .bg .box{
        height: 80px;
        width: 200px;
        background-color: rgba(255, 255, 255, .8);
        margin: 240px auto 0 auto;
        border-radius: 15px;
        text-align: center;
        padding-top: 20px;
        box-shadow: #2c3e50 1px 1px 5px -2px;
    }
    .bg .box button{
        height: 30px;
        line-height: 30px;
        margin: 20px auto 0 auto;
        padding: 0;
        border: none;
        background-color: #2c3e50;
        outline: none;
        width: 80px;
        color: #42b983;
        border-radius: 6px;
    }
    .bg .box button:hover{
        background-color: #1f2b38;
    }
</style>