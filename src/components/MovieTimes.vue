<template>
  <div>
    <!-- Header -->
    <section class="section is-link is-fullheight is-centered" id="#movietimes">
      <div>
        <h1 class="title is-1 has-text-white" style="clear:both">
          Movie Times
        </h1>
        <div v-for="result in results">
          <div class="card">
            <div class="card-content">
              <p class="subtitle has-text-weight-semibold">
                {{ result.title }}
              </p>
            </div>
            <footer class="card-footer">
              <p class="card-footer-item" v-for="el in result.times">
                <span>{{ el }}</span>
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
    getMovies(url) {
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
      this.getMovies(API + this.IDtoSearch);
    }
  },
  beforeMount() {
    this.buildUrl();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$cyan: hsl(204, 86%, 53%);
$card-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);

section {
  display: flex;
  background-color: $cyan;
  min-height: 100vh;
  justify-content: center;
}

.card {
  box-shadow: $card-shadow;
}

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
