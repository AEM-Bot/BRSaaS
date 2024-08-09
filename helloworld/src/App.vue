<template>
  <!--  <img alt="Vue logo" src="./assets/logo.png">-->
  <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
.hello {
  text-align: center;
  font-weight: 900;
  font-size: 300%;
  margin-left: 519px;
  min-height: 120px;
  width: 800px;
  padding-top: 8%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
