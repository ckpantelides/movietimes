<template>
  <div class="hello">
    <!-- Header -->
    <section class="section is-link is-fullheight" id="#cinemas">
      <div>
        <h1 class="title is-1 has-text-white" style="clear:both">Cinemas</h1>
        <div v-for="result in results">
          <div class="card" @click="cinemaChosen(result.id)">
            <div class="card-content">
              <p class="subtitle has-text-weight-semibold">{{ result.name }}</p>
            </div>
            <footer class="card-footer">
              <p class="card-footer-item">
                <span>{{ result.distance }} km</span>
              </p>
            </footer>
          </div>
          <br />
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
          console.log("Error with coordinate search");
          /****************************************************/
          // Need to catch error with this too
          this.getCinemas(
            "https://api.cinelist.co.uk/search/cinemas/location/finchley"
          );
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
      this.$emit("cinemaChosen", cinemaID);
    }
  },
  beforeMount() {
    this.geolocation();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$primary: hsl(171, 100%, 41%);
$card-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);

section {
  background-color: $primary;
  display: flex;
  justify-content: center;
}

.card {
  box-shadow: $card-shadow;
  // max-width: 500px;
}

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
