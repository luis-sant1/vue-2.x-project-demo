<template>
  <div id="app">
    <LandingPage />
  </div>
</template>

<script>
import LandingPage from './components/Landing.vue';
export default {
  name: 'App',
  data() {
    return {message: 'Hello World'}
  } ,
  components: {
    LandingPage
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
