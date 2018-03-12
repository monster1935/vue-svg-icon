<template>
  <div id="app">
    <div class="header">
      <img src="./assets/logo.png">
      <h1>{{ msg }}</h1>
    </div>
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
export default {
  name: 'App',
  data() {
    return {
        msg: 'Vue Svg Icon Demo'
    }
  },
  components: {
    HelloWorld,
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 1000px;
  margin: 0 auto;
}
.header {
  text-align: center;
  margin-bottom: 60px;
}
</style>
