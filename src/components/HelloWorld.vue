<template>
  <div class="hello">
    <!-- Header -->
    <section class="section is-link is-fullheight" id="home">
      <div>
        <h1 class="title is-1" style="clear:both">Movie times</h1>
        <div v-for="result in results">
          <p>{{ result.name }}</p>
          <p>{{ result.distance }} km</p>
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
  name: "HelloWorld",
  props: {
    msg: String
  },
  data: function() {
    return {
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
    }
  },
  beforeMount() {
    this.geolocation();
  }
};
</script>
<!--

    mounted() {
    axios.get("https://api.cinelist.co.uk/search/cinemas/postcode/n32sg")
    .then(response => {this.results = response.data.cinemas})
  }

        {
          postcode: "WC2R0RN",
          cinemas: [
            {
              name: "Cineworld London - Leicester Square, Camden Town",
              id: "10539",
              distance: 0.58
            },
            {
              name: "Odeon London Covent Garden , Camden Town",
              id: "9869",
              distance: 0.58
            },
            {
              name: "Odeon London Leicester Square - ODEON Luxe, Camden Town",
              id: "10716",
              distance: 0.58
            }
          ]
        }
        -->
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
