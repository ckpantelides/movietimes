<template>
  <div class="hello">
    <!-- Header -->
    <section class="section is-link is-fullheight" id="#cinemas">
      <!-- Searchbar -->
      <div class="search-container">
        <div class="input-icon">
          <input
            class="input is-small"
            type="text"
            v-model="location"
            placeholder="City, town or placename"
          />
          <span>
            <i
              class="fas fa-search"
              @click="saveLocation()"
              style="color:white; vertical-align:middle"
            ></i>
          </span>
        </div>
      </div>
      <div class="cinemaCards">
        <h1 class="title is-1 has-text-white" style="clear:both">Cinemas</h1>
        <!-- Loader -->
        <div class="loader" v-if="loader"></div>
        <!-- Cinema search results -->
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
      results: [],
      location: "",
      loader: true
    };
  },
  methods: {
    getCinemas(url) {
      axios
        .get(url)
        .then(response => {
          let firstTenResults = response.data.cinemas.slice(0, 10);
          this.results = firstTenResults;
          this.loader = false;
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
    },
    saveLocation() {
      this.$emit("newCinemaSearch", this.location);
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
  min-height: 100vh;
}

.search-container {
  width: 100vw;
}

.input-icon {
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
}

.input.is-small {
  margin-bottom: 0.5rem;
}

div.cinemaCards {
  width: 100vw;
}

.card {
  box-shadow: $card-shadow;
  max-width: 450px;
  margin-left: auto;
  margin-right: auto;
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

.loader {
  transform: translate(-50%, -50%);
  border: 10px solid #f3f3f3; /* Light grey */
  border-top: 8px solid #3498db; /* Blue */
  border-radius: 50%;
  width: 45px;
  height: 45px;
  animation: spin 2s linear infinite;
  margin-right: auto;
  margin-left: auto;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@media screen and (max-width: 768px) {
  section {
    padding: 1.5rem 0;
  }

  .card {
    max-width: 95vw;
  }

  .input-icon {
    max-width: 95vw;
  }
}
</style>
