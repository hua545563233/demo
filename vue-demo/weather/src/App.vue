<template>
  <div id="app">
    <h2>全国各地市天气查询</h2>
    <weather/>
  </div>
</template>

<script>
import Weather from './components/main'

export default {
  name: 'App',
  components: {
    Weather
  }
}
</script>

<style>
body {
  background: #eaf6f9;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
