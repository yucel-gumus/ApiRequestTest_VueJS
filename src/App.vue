<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <RequestApi msg="Api Request Testine Hoşgeldiniz" />
</template>

<script>
import RequestApi from './components/RequestApi.vue'
export default {
  name: 'App',
  components: {
    RequestApi
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
