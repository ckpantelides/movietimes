<template>
  <div>
    <!-- Header -->
    <section class="section is-link is-centered" id="#today">
      <div>
        <div v-for="(result, index) in results" v-bind:key="index">
          <div class="card">
            <div class="card-content">
              <div class="media">
                <div class="media-left">
                  <figure class="image is-128x128">
                    <img :src="images[index]" alt="Placeholder image" />
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
          <br />
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import io from "socket.io-client";

// socket connects client to server for movie image search
var socket = io("http://localhost:8000");
const API = "https://api.cinelist.co.uk/get/times/cinema/";

export default {
  name: "MovieDay0",
  props: {
    IDtoSearch: Number
  },
  data: function() {
    return {
      images: [],
      results: []
    };
  },
  methods: {
    getMovies(url) {
      axios
        .get(url)
        .then(response => {
          this.results = response.data.listings;
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

//new for socket
// hides bottom of image poster
.media {
  overflow-y: hidden;
  height: 100%;
}
img {
  //min-height: 160px;
  //width: auto;
  // overflow-y: hidden;
}

//new for socket
.card-content {
  padding: 0;
}

// new for socket
p.title.is-4 {
  padding: 2rem;
  text-align: center;
}

.card {
  box-shadow: $card-shadow;
  // this is new with images
  min-width: 300px;
  max-width: 500px;
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
