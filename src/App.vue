<template>
  <AppQuestion
    :question="data[count]"
    :counter="`${count + 1}/${data.length}`"
    @next="count < data.length - 1 ? count++ : null"
    @prev="count > 0 ? count-- : null"
  />
</template>

<script>
import { ref } from 'vue'
import AppQuestion from './components/AppQuestion.vue'
import data from './assets/data.json'

export default {
  name: 'App',
  components: {
    AppQuestion
  },
  setup() {
    const count = ref(0)
    return {
      data: Object.values(data),
      count,
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
