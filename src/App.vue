<template>
  <div id="app">
    <stacks></stacks>
  </div>
</template>

<script type="text/ecmascript-6">
import Stacks from './components/stack_basic'
export default {
  components:{
    Stacks
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
