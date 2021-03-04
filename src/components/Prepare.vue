<template>
  <div class="container">
    <h1>Prepare</h1>
    <button @click="toNext">start</button>
  </div>
</template>

<script>
export default {
  name: 'Prepare',
  props: {
    msg: String
  },
  methods:{
      toNext: function(){
          this.$emit('toNext','progress');
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
