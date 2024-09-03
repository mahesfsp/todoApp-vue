<template>
  <div id="app">
    <ToDoApp />
  </div>
</template>

<script>
import ToDoApp from './components/ToDoApp.vue';

export default {
  components: {
    ToDoApp,
  },
};
</script>

<style>
/* Optional global styles */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
