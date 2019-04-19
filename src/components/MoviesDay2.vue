<template>
  <div>
    <!-- Header -->
    <section class="section is-link is-centered" id="#dayafter">
      <div>
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
  name: "MoviesDay2",
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
      this.getMovies(API + this.IDtoSearch + "?day=2");
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
  justify-content: center;
  padding: 0;
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
