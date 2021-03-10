<template>
  <v-app> 
    <component :is="currentComponent" @toNext="toNext" :board="board"></component>
  </v-app>
</template>

<script>

import Welcome from "./components/Welcome.vue";
import Prepare from "./components/Prepare.vue";
import Progress from "./components/Progress.vue";
import Finished from './components/Finished.vue';

import board from "./assets/data/board.json";


export default {
  name: "App",
  components: {
    Welcome,
    Prepare,
    Progress,
    Finished,
  },
  data() {
    return {
      currentComponent: "Welcome",
      board: board
    };
  },
  methods: {
    toNext: function (page) {
      console.log(page);
      this.currentComponent = page;
    }
  },
};
</script>

<style>
@import "./assets/css/reset.css";
@import "./assets/css/style.css";
</style>
