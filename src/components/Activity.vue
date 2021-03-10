<template>
  <div class="activity">
    <p class="activity__detail">
      {{ currentActivity.activity }}
    </p>
    <img
      :src="getIcon(currentActivity.icon)"
      alt="icon"
      width="150"
      height="150"
      class="activity__icon"
    />
    <p class="activity__detail">({{ currentActivity.time }} minuten)</p>
  </div>
</template>

<script>
export default {
  name: "Activity",
  props: {
    currentActivity: Object,
  },
  mounted() {
      console.log('mounted the activity');
      console.log(this.currentActivity);
  },
  methods: {
    getIcon(path) {
      return require("../assets/data/icons/" + path);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.activity {
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: stretch;
  border: 0.1rem solid black;
  margin: 1rem 0;
}

.activity__detail {
  text-align: center;
  font-weight: 700;
  padding: 0.2rem;
  margin: 0.3rem;
  background-color: black;
  color: white;
}

.activity__icon {
  border: 0.1rem solid black;
  padding: 0.2rem;
  margin: 0 auto;
}
</style>
