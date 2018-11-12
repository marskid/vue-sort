<template>
  <div id="app">
    <quick-sort-test :count="count" :min="minRange" :max="maxRange" :duration="duration"/>
    <div class="control-panel">
      <form action="#">
        <input type="button" :disabled="count < minCount" value="decrease" @click="decrease" />
        <input type="number" readonly :value="count" />
        <input type="button" :disabled="count > maxCount" value="increase" @click="increase" />
        <input type="hidden" />
        <input type="button" value="sort" @click="sort" />
        <input type="range" max="1000" min="0" v-model.number="duration" style="vertical-align: bottom;" />
      </form>
    </div>
  </div>
</template>

<script>
import QuickSortTest from './components/QuickSortTest.vue'
import EventBus from './EventBus'

export default {
  name: 'app',
  components: {
    QuickSortTest
  },
  data() {
    return {
      count: 100,
      minRange: 10,
      maxRange: 100,
      minCount: 30,
      maxCount: 250,
      duration: 100
    }
  },
  methods: {
    increase() {
      if (this.count >= this.maxCount) return
      this.count += 10
      this.maxRange += 10
    },
    decrease() {
      if (this.count <= this.minCount) return
      this.count -= 10
      this.maxRange -= 10
    },
    sort() {
      EventBus.$emit('sort')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
