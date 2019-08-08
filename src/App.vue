<template>
  <div id="app">
    <!-- <img id="github-vue-image" alt="Github Vue Image" src="./assets/github-vue.png"> -->
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style>
  @import './styles/styles.css';
</style>
