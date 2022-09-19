<template>
  <div id="app">
    <div id="svgcontainer"></div>
  </div>
</template>

<script>
import * as d3 from 'd3'
import Vue from 'vue'

export default {
  name: 'D3chart3',
  mounted() {
    Vue.nextTick(() => {
      const width = 500
      const height = 200
      const svg = d3
        .select('#svgcontainer')
        .append('svg')
        .attr('width', width)
        .attr('height', height)
      svg
        .append('rect')
        .attr('x', 20)
        .attr('y', 20)
        .attr('width', 500)
        .attr('height', 100)
        .attr('fill', 'green')
    })
  },
}
</script>

<style scoped>

</style>
