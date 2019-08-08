<template>
  <div class="wrapper" ref="aaaa">
    
  </div>
</template>

<script>
  export default {
    name: "Category",
    data() {
      return {
        scroll: null
      }
    },
  }
</script>

<style scoped>
  .wrapper {
    height: 150px;
    background-color: red;

    overflow: hidden;
    /*overflow-y: scroll;*/
  }
</style>
