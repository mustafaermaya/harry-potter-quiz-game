<template>
  <div class="main-page">
    <app-game-component></app-game-component>
  </div>
</template>

<script>
import GameComponent from "./components/GameComponent";
export default {
  name: "app",
  components: {
    appGameComponent: GameComponent
  }
};
</script>

<style>
</style>
