<template>
  <div>
    <!-- Loader -->
    <div class="loader" v-if="loader"></div>
    <!-- Header -->
    <section class="section is-link is-centered" id="#today">
      <!--
         <vue-flip :active-click="true">
           <div slot="front">
           <img :src="images[index].poster" alt="Placeholder image" />
      <p class="blurb">{{ images[index].blurb }}</p>-->
      <div class="card-container">
        <div v-for="(result, index) in results" v-bind:key="index">
          <vue-flip :active-click="true" style="clear:both">
            <div slot="front">
              <div class="card">
                <div class="card-content">
                  <div class="media">
                    <div class="media-left">
                      <figure class="image is-128x128">
                        <img
                          :src="images[index].poster"
                          alt="Placeholder image"
                        />
                      </figure>
                    </div>
                    <div class="media-content">
                      <p class="title is-4">{{ result.title }}</p>
                    </div>
                  </div>
                </div>
                <footer class="card-footer">
                  <p class="card-footer-item" v-for="el in result.times">
                    <span>{{ el }}</span>
                  </p>
                </footer>
              </div>
            </div>
            <div slot="back">
              <div class="card" style="padding: 30px 0px">
                <p class="blurb">{{ images[index].blurb }}</p>
              </div>
              <br />
            </div>
          </vue-flip>
        </div>
        <br />
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import io from "socket.io-client";
import VueFlip from "vue-flip";

// socket connects client to server for movie image search
var socket = io("http://localhost:8000");
// var socket = io("https://movietime-server.herokuapp.com/");
const API = "https://api.cinelist.co.uk/get/times/cinema/";

export default {
  name: "MovieDay0",
  components: {
    "vue-flip": VueFlip
  },
  props: {
    IDtoSearch: Number
  },
  data: function() {
    return {
      images: [],
      results: [],
      loader: true
    };
  },
  methods: {
    getMovies(url) {
      axios
        .get(url)
        .then(response => {
          this.results = response.data.listings;
          this.loader = false;
          // data emitted to server, so server can perform movie image search
          socket.emit("request images", { data: response.data.listings });
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
  },
  mounted() {
    socket.on("image links", data => {
      this.images = data;
    });
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

// hides bottom of image poster
.media {
  overflow-y: hidden;
  height: 100%;
}

.card-content {
  padding: 0;
}

p.title.is-4 {
  padding: 2rem;
  text-align: center;
}

p.blurb {
  font-size: 0.9rem;
}

.card-container {
  // display: flex;
  width: 450px;
  height: 100vh;
}

.card {
  box-shadow: $card-shadow;
  width: 450px;
  // min-width: 350px;
  //  max-width: 500px;
}

footer {
  max-width: 500px;
  overflow-x: auto;
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

.loader {
  transform: translate(-50%, -50%);
  border: 10px solid #f3f3f3; /* Light grey */
  border-top: 8px solid hsl(171, 100%, 41%); /* Green */
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

  .card,
  footer {
    max-width: 95vw;
  }
}
</style>
