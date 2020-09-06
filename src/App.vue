<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <PackChart :tweetData="loadData" />
  </div>
</template>

<script>
import PackChart from './components/PackChart.vue'

import * as d3 from 'd3'

export default {
  name: 'App',
  components: {
    PackChart
  },
  data() {
    return {
      loadData: {}
    }
  },
  methods: {
    async fecthData() {
      let data = await d3.json('./data.json')
      console.log('data')
      console.log(data)
      this.loadData = data
    }
  },
  mounted() {
    console.log('App loaded')
    this.fecthData()
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
