<template>
  <div ref="container" class="container">
    <svg class="line-chart">
      <path
        class="path"
        :d="chartPath"
        fill="none"
        stroke="steelblue"
        :stroke-width="1.5"
        stroke-linejoin="round"
        stroke-linecap="round"
      ></path>
      <g
        class="xAxis"
        :transform="`translate(0, ${height - margin.bottom})`"
        fill="none"
        font-size="10"
        font-family="sans-serif"
        text-anchor="middle"
      >
        <path class="doamin" stroke="currentColor" :d="`M${margin.left},0H${width - margin.left}`"></path>
        <g
          v-for="(v, i) in xTicks"
          :key="`xAxis-tick-${i}`"
          class="tick"
          opacity="1"
          :transform="`translate(${x(v)},0)`"
        >

          <line stroke="currentColor" y2="6"></line>
          <text fill="currentColor" y="9" dy="0.71em">
            {{ parseTime(v) }}
          </text>
        </g>
      </g>
      <g
        class="yAxis"
        :transform="`translate(${margin.left}, 0)`"
        fill="none"
        font-size="10"
        font-family="sans-serif"
        text-anchor="end"
      >
        <g
          v-for="(v, i) in yTicks"
          :key="`xAxis-tick-${i}`"
          class="tick"
          opacity="1"
          :transform="`translate(0,${y(v)})`"
        >
          <line stroke="currentColor" x2="-6"></line>
          <text fill="currentColor" x="-9" dy="0.32em">
            {{ v }}
          </text>
          <text v-if="i === yTicks.length - 1" fill="currentColor" x="3" dy="0.32em" text-anchor="start" font-weight="bold">$ Close</text>
        </g>
      </g>
    </svg>
  </div>
</template>

<script>
import * as d3 from 'd3'
import { selectAll } from 'd3-selection'
import data from '../../assets/data.json'

export default {
  name: 'LineChart2',
  data() {
    return {
      width: 0,
      height: 0,
      margin: {
        top: 20,
        right: 30,
        bottom: 30,
        left: 40,
      },
    }
  },
  computed: {
    svgWidth: {
      get() {
        return this.width
      },
      set(element) {
        this.width = element.getBoundingClientRect().width
      },
    },
    svgHeight: {
      get() {
        return this.height
      },
      set(element) {
        this.height = element.getBoundingClientRect().height
      },
    },
    parseTime() {
      const formatMonth = d3.timeFormat('%B')
      const formatYear = d3.timeFormat('%Y')

      return date => {
        if (date.getMonth() === 0) {
          return formatYear(date)
        }

        return formatMonth(date)
      }
    },
    yTicks() {
      return this.y.ticks()
    },
    xTicks() {
      return this.x.ticks(this.width / 80)
    },
    chartPath() {
      return this.line(this.chartData)
    },
    chartData() {
      return Object.assign(data.map(({ date, value }) => ({ date: new Date(date), value })))
    },
    x() {
      return d3.scaleUtc()
        .domain(d3.extent(this.chartData, d => d.date))
        .range([this.margin.left, this.width - this.margin.right])
    },
    y() {
      return d3.scaleLinear()
        .domain([0, d3.max(this.chartData, d => d.value)]).nice()
        .range([this.height - this.margin.bottom, this.margin.top])
    },
    line() {
      return d3.line()
        .defined(d => !Number.isNaN(d.value))
        .x(d => this.x(d.date))
        .y(d => this.y(d.value))
    },
  },
  mounted() {
    this.resize()
    this.AnimateLoad()
    window.addEventListener('resize', this.resize)
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.resize)
  },
  methods: {
    resize() {
      this.svgWidth = this.$refs.container
      this.svgHeight = this.$refs.container
      console.log(this.$refs.container)
    },
    AnimateLoad() {
      // const path = d3
      //   .select('svg')
      //   .append('path')
      //   .attr('d', this.line(this.chartData))
      //   .attr('fill', 'none')
      //   .attr('stroke-width', 2)
      //   .attr('stroke', 'blue')
      // const pathLength = path.node().getTotalLength()
      // console.log(pathLength)
      selectAll('.path')
        .data(this.chartData)
        .transition()
        .ease(d3.easeLinear)
        .duration(2500)
        .attrTween('stroke-dasharray', function() {
          const length = this.getTotalLength()
          return d3.interpolate(`0,${length}`, `${length},${length}`)
        })
    },
  },
}
</script>

<style scoped>
.container {
  height: 0;
  width: 100%;
  padding-top: 30%;
  position: relative;
  max-height: 300px;
}
.line-chart {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
/*.line-chart:hover path {*/
/*  d: path("M66.039,133.545c0,0-21-57,18-67s49-4,65,8 s30,41,53,27s66,4,58,32s-5,44,18,57s22,46,0,45s-54-40-68-16s-40,88-83,48s11-61-11-80s-79-7-70-41 C46.039,146.545,53.039,128.545,66.039,133.545z");*/
/*}*/
/*.path {*/
/*  stroke-dasharray: 1000;*/
/*  stroke-dashoffset: 1000;*/
/*  animation: dash 10s linear alternate infinite;*/
/*}*/

/*@keyframes dash {*/
/*  from {*/
/*    stroke-dashoffset: 1000;*/
/*  }*/
/*  to {*/
/*    stroke-dashoffset: 10000;*/
/*  }*/
/*}*/
</style>
