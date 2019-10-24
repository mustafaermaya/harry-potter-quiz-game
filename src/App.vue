<template>
<div>
  <app-questions></app-questions>
</div>
</template>

<script>
import QuestionsComponent from './components/QuestionsComponent'
export default {
  name: 'app',
  components: {
    appQuestions : QuestionsComponent
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
