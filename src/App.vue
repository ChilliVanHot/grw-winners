<template>
  <div id="app">
    <img src="./assets/grw-logo.png">
    <HelloWorld />
  </div>
</template>

<script>
//Yes - I used the hello world template as this is my first vueJS project
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
    tableData: [],
    sortKey: 'name',
    reverse: false,
    search: '',
    orderedList: [],
    components: {
       HelloWorld
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
