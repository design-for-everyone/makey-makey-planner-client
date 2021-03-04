<template>
  <div class="container">
    <h1>Welcome</h1>
    <button @click="toNext">start</button>
  </div>
</template>

<script>
export default {
  name: 'Welcome',
  props: {
    msg: String
  },
  methods:{
      toNext: function(){
          this.$emit('toNext','prepare');
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
