<template>
  <div id="app">
    <component
      :is="currentComponent"
      @cinemaChosen="loadCinemaComponent"
      :IDtoSearch="cinemaIDprop"
    ></component>
  </div>
</template>

<script>
import Cinemas from "./components/Cinemas.vue";
import MovieTimes from "./components/MovieTimes.vue";

export default {
  name: "app",
  components: {
    Cinemas,
    MovieTimes
  },
  data: function() {
    return {
      currentComponent: Cinemas,
      cinemaIDprop: Number
    };
  },
  methods: {
    loadCinemaComponent(payload) {
      this.cinemaIDprop = parseInt(payload);
      this.currentComponent = MovieTimes;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
