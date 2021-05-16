<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <Page :pageLength="16" @switchPage="switchPage"></Page>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Page from './components/Page.vue'


export default {
  name: 'App',
  components: {
    HelloWorld,
    Page
  },
  methods:{
      switchPage(index){
          console.log(index);
      }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
