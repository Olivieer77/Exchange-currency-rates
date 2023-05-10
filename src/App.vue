<template>
  <div id="app">
    <ButtonsComp />
  </div>
</template>

<script>
import ButtonsComp from './components/ButtonsComp.vue'
export default {
  name: 'App',
  components: {
    ButtonsComp,
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
