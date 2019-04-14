<template>
  <div class="hello">
    <!-- Header -->
    <section class="section is-link is-fullheight" id="cinemas">
      <div>
        <h1 class="title is-1" style="clear:both">Cinemas</h1>
        <div v-for="result in results">
            <div @click="cinemaChosen(result.id)">
              <p>{{ result.name }}</p>
              <p>{{ result.distance }} km</p>
            </div>       
          </br>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
const API = "https://api.cinelist.co.uk/search/cinemas/coordinates/";

export default {
  name: "Cinemas",
  props: {
    msg: String
  },
  data: function() {
    return {
      cinemaID: Number,
      results: []
    };
  },
  methods: {
    getCinemas(url) {
      axios
        .get(url)
        .then(response => {
          let firstTenResults = response.data.cinemas.slice(0, 10);
          this.results = firstTenResults;
        })
        .catch(error => {
          console.log(error);
        });
    },
    geolocation() {
      navigator.geolocation.getCurrentPosition(this.buildUrl, this.geoError);
    },
    buildUrl(position) {
      const lat = position.coords.latitude;
      const lon = position.coords.longitude;

      this.getCinemas(API + lat + "/" + lon);
    },
    geoError() {
      this.getCinemas(API + "51.510357/-0.116773");
    },
    cinemaChosen(cinemaID) {
    this.$emit('cinemaChosen', cinemaID)
    }
  },
  beforeMount() {
    this.geolocation();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
