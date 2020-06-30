<template>
  <div id="app">
    <comp-game/>
  </div>
</template>

<script>
import CompGame from './Components/CompGame.vue';
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  components : {
    CompGame
  }
}
</script>

<style>
</style>
