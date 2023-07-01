<template>
  <div id="app">
    <PokemonJuego />
  </div>
</template>

<script>
import PokemonJuego from "./components/PokemonJuego.vue";

export default {
  components: {
    PokemonJuego,
  },
};
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
