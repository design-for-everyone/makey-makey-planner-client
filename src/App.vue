<template>
  <div id="app">
    <Welcome @toNext="toNext" v-show="currentStatus === 'welcome'" />
    <Prepare @toNext="toNext" v-show="currentStatus === 'prepare'"/>
    <Progress @toNext="toNext" v-show="currentStatus === 'progress'" />
    <hr>
    <p>{{currentStatus}}</p>
  </div>
</template>

<script>
import Welcome from "./components/Welcome.vue";
import Prepare from "./components/Prepare.vue";
import Progress from "./components/Progress.vue";

//const statuses = ['welcome','prepare','progress'];

export default {
  name: "App",
  components: {
    Welcome,
    Prepare,
    Progress,
  },
  data() {
    return {  
      currentStatus: 'welcome'
      };
  },
  methods: {
    toNext: function(page){
      this.currentStatus = page
    }
  }
};
</script>

<style>
@import "./assets/css/reset.css";
@import "./assets/css/style.css";
</style>
