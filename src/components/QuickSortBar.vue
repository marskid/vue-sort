<template>
  <div class="bar" :style="style"></div>
</template>

<script>
export default {
  name: 'QuickSortBar',
  props: {
    height: Number
  },
  computed: {
      style() {
          return {
              height: this.height + 'px'
          }
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.bar {
    display: inline-block;
    width: 3px;
    border: 1px solid black;
    border-top: 5px solid red;
    margin: 0 .5px;
    background-color: yellow;
}
.bar.swap {
    background-color: darkred;
}
.bar:hover {
    background-color: midnightblue;
}
.finish > div {
    background-color: darkorange;
}
</style>
