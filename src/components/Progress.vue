<template>
  <div class="container">
    <h1>Progress</h1>
    <button @click="toNext">start</button>
  </div>
</template>

<script>
export default {
  name: "Progress",
  props: {
    msg: String,
  },
  methods: {
    toNext: function () {
      this.$emit("toNext", "welcome");
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
