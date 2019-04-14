<template>
  <div>
    <!-- Header -->
    <section class="section is-link is-fullheight" id="movietimes">
      <div>
        <h1 class="title is-1" style="clear:both">Movie Times</h1>
        <div v-for="result in results">
          <p>{{ result.title }}</p>
          <ul v-for="el in result.times">
            <li>{{ el }}</li>
          </ul>
          </br>
          </br>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
const API = "https://api.cinelist.co.uk/get/times/cinema/";

export default {
  name: "MovieTimes",
  props: {
    IDtoSearch: Number
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
          this.results = response.data.listings;
        })
        .catch(error => {
          console.log(error);
        });
    },
    buildUrl() {
      this.getCinemas(API + this.IDtoSearch);
    }
  },
  beforeMount() {
    this.buildUrl();
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
  display: inline;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
