<template>
  <div id="app">
    <TodoInput msg="Enjoy your tasks ;)"/>
  </div>
</template>

<script>
import TodoInput from './components/TodoInput.vue'

export default {
  name: 'App',
  components: {
    TodoInput
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
}
</style>
