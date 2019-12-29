<template>
  <div id="app">
    <component
      :is="currentComponent"
      @cinemaChosen="loadMovieTimes"
      @navigateHome="reloadCinemas"
      @newCinemaSearch="newCinemaSearch"
      :IDtoSearch="cinemaIDprop"
      :newLocation="newLocation"
    ></component>
  </div>
</template>

<script>
import Cinemas from "./components/Cinemas.vue";
import MovieTimes from "./components/MovieTimes.vue";
import NewCinemaSearch from "./components/NewCinemaSearch.vue";

export default {
  name: "app",
  components: {
    Cinemas,
    MovieTimes,
    NewCinemaSearch
  },
  data: function() {
    return {
      currentComponent: Cinemas,
      cinemaIDprop: String,
      newLocation: String
    };
  },
  methods: {
    loadMovieTimes(payload) {
      this.cinemaIDprop = payload;
      this.currentComponent = MovieTimes;
    },
    reloadCinemas() {
      this.currentComponent = Cinemas;
    },
    newCinemaSearch(data) {
      this.newLocation = data;
      this.currentComponent = NewCinemaSearch;
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
