<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Camera msg="Vue PWA Camera"/>
  </div>
</template>

<script>
import Camera from './components/Camera.vue'

export default {
  name: 'app',
  components: {
    Camera
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
