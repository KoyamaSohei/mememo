<template>
  <Editor />
</template>

<script>
import Editor from './components/editor.vue'

export default {
  name: 'App',
  components: {
    Editor
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin: 60px auto 0 auto;
  max-width: 825px;
  max-height: 100vh;
}
*:focus {
  outline: none;
}
</style>
