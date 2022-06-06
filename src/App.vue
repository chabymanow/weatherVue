<template>
  <Weather main_city="London" />
</template>

<script>
import Weather from './components/Weather.vue'

export default {
  name: 'App',
  components: {
    Weather,
}
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: content-box;
}

body{
  width: 100vw;
  min-height: 90vh;
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
