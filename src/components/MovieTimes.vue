<template>
  <div>
    <!-- Header -->
    <section class="section is-link is-fullheight is-centered" id="#movietimes">
      <span class="icon" @click="navigateHome">
        <i class="fas fa-home" style="color:white"></i>
      </span>
      <br />
      <br />
      <h1 class="title is-1 has-text-white">Movie Times</h1>
      <div class="tabs is-centered">
        <p
          @click="chooseMoviesDay('MoviesDay0')"
          :class="[
            'tab',
            {
              active:
                currentComponent === 'MoviesDay0' ||
                this.componentOpenedFirstTime === true
            }
          ]"
        >
          Today
        </p>
        <p
          @click="chooseMoviesDay('MoviesDay1')"
          :class="['tab', { active: currentComponent === 'MoviesDay1' }]"
        >
          Tomorrow
        </p>
        <p
          @click="chooseMoviesDay('MoviesDay2')"
          :class="['tab', { active: currentComponent === 'MoviesDay2' }]"
        >
          {{ dayAfterTomorrow }}
        </p>
      </div>
      <component :is="currentComponent" :IDtoSearch="IDtoSearch"></component>
    </section>
  </div>
</template>

<script>
import MoviesDay0 from "../components/MoviesDay0.vue";
import MoviesDay1 from "../components/MoviesDay1.vue";
import MoviesDay2 from "../components/MoviesDay2.vue";

export default {
  name: "MovieTimes",
  props: {
    IDtoSearch: Number
  },
  components: {
    MoviesDay0,
    MoviesDay1,
    MoviesDay2
  },
  data: function() {
    return {
      currentComponent: MoviesDay0,
      // Boolean for the styling/underlining of "Today"
      componentOpenedFirstTime: true
    };
  },
  methods: {
    chooseMoviesDay(chosenComponent) {
      this.currentComponent = chosenComponent;
      this.componentOpenedFirstTime = false;
    },
    navigateHome() {
      this.$emit("navigateHome");
    }
  },
  computed: {
    dayAfterTomorrow: function() {
      var d = new Date();
      var dayAfterTom = parseInt(d.getDay() + 2);
      var days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
        "Monday"
      ];
      return days[dayAfterTom];
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$cyan: hsl(204, 86%, 53%);
$card-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);

section {
  background-color: $cyan;
  min-height: 100vh;
}

div.tabs {
  max-width: 300px;
  margin-left: auto;
  margin-right: auto;
}

p.tab.active {
  text-decoration: underline;
}

p.tab {
  color: white;
}

.active {
  text-decoration: underline;
}
h3 {
  margin: 40px 0 0;
}

@media screen and (max-width: 768px) {
  section {
    padding: 1.5rem 0;
  }
}
</style>
