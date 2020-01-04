<template>
  <div>
    <!-- Loader -->
    <div class="loader" v-if="loader"></div>
    <!-- Header -->
    <section class="section is-link is-centered" id="#today">
      <div class="card-container">
        <div v-for="(result, index) in results" v-bind:key="index">
          <vue-flip :active-click="true">
            <div slot="front">
              <div class="card">
                <div class="card-content">
                  <div class="media">
                    <div class="media-left">
                      <figure class="image is-128x128">
                        <img :src="images[index].poster" alt="Placeholder image" />
                      </figure>
                    </div>
                    <div class="media-content">
                      <p class="title is-4">{{ result.name }}</p>
                    </div>
                  </div>
                </div>
                <footer class="card-footer">
                  <p class="card-footer-item" v-for="el in result.schedules[0].performances">
                  <!--  <a :href="el.links[0].url" target="_blank"> -->
                      <span>{{ el.ts.slice(11, 16) }}</span>
                    </a>
                  </p>
                </footer>
              </div>
            </div>

            <div slot="back">
              <div class="card" style="padding: 30px 5px; overflow-y:auto">
                <p class="blurb">{{ images[index].blurb }}</p>
              </div>
            </div>
          </vue-flip>
          <br />
          <div class="break"></div>
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
import mockFilms from "../assets/films.json";

// socket connects client to server for movie image search
//var socket = io("http://localhost:3000");
var socket = io("https://movietime-server.herokuapp.com/");
// const API = "http://localhost:8000/filmtimes";
const API = "https://thelist-api.herokuapp.com/filmtimes";

export default {
  name: "MovieDay0",
  components: {
    "vue-flip": VueFlip
  },
  props: {
    IDtoSearch: String
  },
  data: function() {
    return {
      images: [
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        },
        {
          poster: "~/assets/images/placeholder.png",
          blurb: "Description loading..."
        }
      ],
      unfilteredResults: [],
      results: [],
      loader: true,
      el: {
        links: [
          {
            url: "www.google.com"
          }
        ]
      }
    };
  },
  watch: {
    images: function() {
      this.$forceUpdate();
    }
  },
  methods: {
    getMovies(url) {
      axios
        .get(url, {
          params: {
            cinemaID: this.IDtoSearch
          }
        })
        .then(response => {
          this.unfilteredResults = response.data;
          console.log(response.data);
          this.loader = false;
          this.filterResults();
        })
        .catch(error => {
          console.log(error);
        });
    },
    filterResults() {
      let longDate = new Date();
      var formattedDate = longDate.toISOString().slice(0, 10);

      // only include performances that match the relevant date (above)
      let filteredPerformances = this.unfilteredResults.map(function(
        CompareWithDate
      ) {
        CompareWithDate.schedules[0].performances = CompareWithDate.schedules[0].performances.filter(
          x => x.ts.slice(0, 10) === formattedDate
        );
        return CompareWithDate;
      });
      // filters out films without any performances on the relevant date
      this.results = filteredPerformances.filter(
        obj => obj.schedules[0].performances.length > 0
      );
      this.requestImages();
    },
    requestImages() {
      let movieNames = [];
      let arr = this.results;
      // get an array of the movie array
      for (let i = 0; i < arr.length; i++) {
        movieNames.push(arr[i].name);
      }
      // emits the movie names array to the server, so the server can search for movie posters
      socket.emit("request images", { data: movieNames });
    }
  },
  beforeMount() {
    this.getMovies(API);
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
  height: 128px;
}

.media-content {
  overflow-y: auto;
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
  width: 450px;
}

.card {
  box-shadow: $card-shadow;
  width: 450px;
  margin-bottom: 50px;
  height: 178px;
  // min-width: 350px;
  //  max-width: 500px;
}

// break inserted as cards were overlapping with vue-flip
.break {
  height: 185px;
}

footer {
  max-width: 450px;
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
  border: 7px solid #d4d4d4;
  border-top: 6px solid hsl(204, 78%, 33%); /* Dark blue */
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

  .card-container,
  .card,
  footer {
    width: 95vw;
  }
}
</style>
