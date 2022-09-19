<template>
  <svg :width="width" :height="height">
    <path
      :d="chartPath"
      fill="none"
      stroke="steelblue"
      :stroke-width="1.5"
      stroke-linejoin="round"
      stroke-linecap="round"
    ></path>
<!--    <g-->
<!--      class="xAxis"-->
<!--      :transform="`translate(0, ${height - margin.bottom})`"-->
<!--      fill="none"-->
<!--      font-size="10"-->
<!--      font-family="sans-serif"-->
<!--      text-anchor="middle"-->
<!--    >-->
<!--      <path class="domain" stroke="currentColor" :d="`M${margin.left},0H${width - margin.left}`"></path>-->
<!--      <g-->
<!--        v-for="(v, i) in xTicks"-->
<!--        :key="`xAxis-tick-${i}`"-->
<!--        class="tick"-->
<!--        opacity="1"-->
<!--        :transform="`translate(${x(v)},0)`"-->
<!--      >-->

<!--        <line stroke="currentColor" y2="6"></line>-->
<!--        <text fill="currentColor" y="9" dy="0.71em">-->
<!--          {{ parseTime(v) }}-->
<!--        </text>-->
<!--      </g>-->
<!--    </g>-->
    <path fill="none" stroke="#76BF8A" stroke-width="3" :d="path"></path>
  </svg>
</template>

<script>
import * as d3 from 'd3'

export default {
  name: 'LineChart2',
  data() {
    return {
      data: [
        90, 72, 75, 25, 10, 92,
      ],
      value: [
        'a', 'b', 'c', 'd', 'e', 'f',
      ],
      margin: {
        top: 20,
        right: 30,
        bottom: 30,
        left: 40,
      },
      width: 500,
      height: 300,
      padding: 20,
    }
  },
  computed: {
    path() {
      return this.line(this.data)
    },
    yTicks() {
      return this.y.ticks()
    },
    xTicks() {
      return this.x.ticks(this.width / 80)
    },
    chartPath() {
      return this.line(this.data)
    },
    parseTime() {
      return this.value
    },
    line() {
      return d3
        .line()
        .x((d, i) => this.xScale(i))
        .y(d => this.yScale(d))
    },
    x() {
      return d3.scaleUtc()
        .domain(d3.extent(this.chartData, d => d.data))
        .range([this.margin.left, this.width - this.margin.right])
    },
    y() {
      return d3.scaleLinear()
        .domain([0, d3.max(this.chartData, d => d.value)]).nice()
        .range([this.height - this.margin.bottom, this.margin.top])
    },
    xScale() {
      return d3
        .scaleLinear()
        .range([this.padding, this.width - this.padding])
        .domain(d3.extent(this.data, (d, i) => i))
    },
    yScale() {
      return d3.scaleLinear().range([this.height - this.padding, this.padding]).domain([0, 100])
    },
  },
}
</script>

<style scoped>

</style>
