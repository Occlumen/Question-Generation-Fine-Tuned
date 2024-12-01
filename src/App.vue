<template>
  <QuestionGeneration />
</template>

<script>
import QuestionGeneration from './components/QuestionGeneration.vue'

export default {
  name: 'App',
  components: {
    QuestionGeneration
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
