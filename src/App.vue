<template>
  <div class="main-page">
    <app-questions></app-questions>
  </div>
</template>

<script>
import QuestionsComponent from "./components/QuestionsComponent";
export default {
  name: "app",
  components: {
    appQuestions: QuestionsComponent
  }
};
</script>

<style>
</style>
