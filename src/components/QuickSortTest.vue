<template>
  <div class="content">
    <div id="TestWrapper" :class="{finish : finish}">
      <quick-sort-bar v-for="(value, index) in list" :key="index" :height="value" :class="{swap : index == indexA || index == indexB}" />
    </div>
  </div>
</template>

<script>
import Random from 'random-js'
import QuickSortBar from './QuickSortBar.vue'
import EventBus from '../EventBus'

let random = new Random()

export default {
  name: 'QuickSortTest',
  props: {
    count: Number,
    min: {
      type: Number,
      default: 10
    },
    max: {
      type: Number,
      default: 100
    },
    duration: {
      type: Number,
      default: 100
    }
  },
  components: {
    QuickSortBar
  },
  watch: {
    count() {
      this.list = this.random()
      this.finish = false
      this.cancle = true
      this.indexA = -1
      this.indexB = -1
    },
    finish() {
      if (this.finish) {
        this.indexA = -1
        this.indexB = -1
      }
    }
  },
  data() {
    return {
      list: this.random(),
      finish: false,
      indexA: -1,
      indexB: -1,
      cancle: false
    }
  },
  methods: {
    random() {
      let list = [], num = this.count
      while(num--) {
        list.push(random.integer(this.min, this.max))
      }
      return list
    },
    async sort() {
      /* eslint-disable */
      let vm = this, list = this.list, length = list.length
      let swap = (a, b, list) => {
        let temp = list[a]
        vm.indexA = a
        vm.indexB = b
        vm.$set(list, a, list[b])
        vm.$set(list, b, temp)
        return new Promise(resolve => setTimeout(resolve, vm.duration))
      }
      this.finish = false
      this.cancle = false
      
      for (let i =  0; i < length; i ++) {
        for (let j = i; j < length; j ++) {
          if (this.cancle) break

          if (list[i] > list[j]) {
            await swap(i, j, list)
          }
        }
      }
      if (!this.cancle) {
        this.finish = true
      }
    }
  },
  mounted() {
    let vm = this
    EventBus.$on('sort', () => {
      if(vm.finish) {
        vm.list = vm.random()
      }
      vm.sort()
    })
  }
}
</script>
