<template>
  <v-app>
    <component
      :is="currentComponent"
      @toNext="toNext"
      :board="board"
      :ip="serverIp"
    ></component>
  </v-app>
</template>

<script>
import Welcome from "./components/Welcome.vue";
import Prepare from "./components/Prepare.vue";
import Progress from "./components/Progress.vue";
import Finished from "./components/Finished.vue";

import axios from "axios";

import board from "./assets/data/board.json";

export default {
  name: "App",
  components: {
    Welcome,
    Prepare,
    Progress,
    Finished,
  },
  mounted() {
    // get the ip of the server and pass it to the components
    axios
      .get(`./status`)
      .then((response) => {
        // JSON responses are automatically parsed.
        this.serverIp = response.data.ip;
      })
      .catch((e) => {
        console.log(e);
      });
  },
  data() {
    return {
      currentComponent: "Welcome",
      board: board,
      serverIp: "",
    };
  },
  methods: {
    toNext: function (page) {
      console.log(page);
      this.currentComponent = page;
    },
  },
};
</script>

<style>
@import "./assets/css/reset.css";
@import "./assets/css/style.css";
</style>
