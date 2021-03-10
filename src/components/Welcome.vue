<template>
  <div class="container">
    <h1 class="message">Het bord is momenteel niet in gebruik</h1>
    <v-btn x-large color="primary" @click="toNext">maak nieuw bord</v-btn>
  </div>
</template>

<script>
export default {
  name: "Welcome",
  methods: {
    toNext: function () {
      document.documentElement.requestFullscreen();
      this.$emit("toNext", "Prepare");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: center;
}

.message {
  font-size: 2rem;
  margin-bottom: 1rem;
}
</style>
