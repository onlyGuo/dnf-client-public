<template>
    <div id="app">
        <title-bar text="登陆器"/>
        <router-view/>
    </div>
</template>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #42b983;
    background-color: #2c3e50;
    height: 100%;
}
html, body{
    padding: 0;
    margin: 0;
    height: 100%;
}
</style>
<script>
import TitleBar from "./components/TitleBar";
export default {
    components: {TitleBar}
}
</script>yarn