<template>
  <div>
    <svg width="350" height="350">
      <path class="path" fill="white" stroke="black" stroke-width="4" d="M66.039,133.545c0,0-21-57,18-67s49-4,65,8
  s30,41,53,27s66,4,58,32s-5,44,18,57s22,46,0,45s-54-40-68-16s-40,88-83,48s11-61-11-80s-79-7-70-41
  C46.039,146.545,53.039,128.545,66.039,133.545z"/>
    </svg>
  </div>
</template>

<script>
import * as d3 from 'd3'

export default {
  name: 'LIneChart3',
  data() {
    return {
      data: [
        { value: 3, time: new Date('2020-07-01T03:00:00') },
        { value: 1, time: new Date('2020-07-01T03:05:00') },
        { value: 9, time: new Date('2020-07-01T03:10:00') },
        { value: 6, time: new Date('2020-07-01T03:15:00') },
        { value: 2, time: new Date('2020-07-01T03:20:00') },
        { value: 6, time: new Date('2020-07-01T03:25:00') },
      ],
    }
  },
  computed: {
    chartPath2() {
      return this.line(this.chartData)
    },
    xScale() {
      return d3
        .scaleTime()
        .domain([
          new Date('2020-07-01T03:00:00'),
          new Date('2020-07-01T03:25:00'),
        ])
        .range([20, 330])
    },
    yScale() {
      return d3
        .scaleLinear()
        .domain([1, 9])
        .range([330, 20])
    },
    line() {
      return d3
        .line() // 들어온 데이터 값을 완성된 'd' 속성의 값으로 반환해주는 함수를 만드는 함수이다.
        .x(d => this.xScale(d.time))
        .y(d => this.yScale(d.value))
    },
  },
  mounted() { // react에서는 componentDidmount 실행시 축을 생성해주면 된다.
    this.generateAxis()
  },
  methods: {
    generateAxis() {
      const xScale = d3
        .scaleTime()
        .domain([
          new Date('2020-07-01T03:00:00'),
          new Date('2020-07-01T03:25:00'),
        ])
        .range([20, 330])

      const yScale = d3
        .scaleLinear()
        .domain([1, 9])
        .range([330, 20]) // SVG 좌표상에서 y값이 높을수록 아래로 향하기 때문에 뒤집어서 330~20으로 설정.

      const xAxisSVG = d3.select('svg').append('g').attr('transform', 'translate(0,330)') // attr속성을 이용하면 선택한 태그들의 속성을 바꿀 수 있다.
      const yAxisSVG = d3.select('svg').append('g')

      // 축을 만드는 함수를 만든다.
      const xAxis = d3
        .axisBottom(xScale)
        .tickSize(10)
        .ticks(10) // 여기서 tick은 축마다 달려있는 막대기를 말한다.
      const yAxis = d3
        .axisRight(yScale)
        .tickSize(10)
        .ticks(10)
      xAxis(xAxisSVG) // x축을 만드는 함수로 SVG > G 태그에 축을 생성한다.
      yAxis(yAxisSVG) // y축을 만드는 함수로 SVG > G 태그에 축을 생성한다.

      // d3.select('svg').selectAll('circle') // 1.SVG 태그 안에 있는 circle을 모두 찾는다.
      //   .data(this.data) // 2.찾은 요소에 데이터를 씌운다.
      //   .enter() // 3.찾은 요소에 개수보다 데이터가 더 많을경우..
      //   .append('circle') // 4.circle 을 추가한다.
      //   .attr('r', 5) //  - 반지름 5픽셀
      //   .attr('cx', d => xScale(d.time)) //  - x 위치값 설정.
      //   .attr('cy', d => yScale(d.value)) //  - y 위치값 설정.
      //   .style('fill', 'black') //  - 검정색

      const linearGenerator = d3
        .line() // 들어온 데이터 값을 완성된 'd' 속성의 값으로 반환해주는 함수를 만드는 함수이다.
        .x(d => xScale(d.time))
        .y(d => yScale(d.value))

      console.log(linearGenerator)

      // d3.select('svg')
      //   .append('path') // SVG 태그 안에 path 속성을 추가한다.
      //   .attr('d', linearGenerator(this.data)) // - 라인 생성기로 'd' 속성에 들어갈 좌표정보를 얻는다.
      //   .attr('fill', 'none') // - 라인 안쪽 채우지 않음.
      //   .attr('stroke-width', 2) // - 굵기
      //   .attr('stroke', 'blue') // - 파랑색

      const path = d3
        .select('svg')
        .append('path')
        .attr('d', linearGenerator(this.data))
        .attr('fill', 'none')
        .attr('stroke-width', 2)
        .attr('stroke', 'blue')

      // path의 총길이를 구한다.
      const pathLength = path.node().getTotalLength()
      console.log(pathLength)

      // d3에서 제공하는 transition 함수
      const transitionPath = d3
        .transition()
        .ease(d3.easeSin)
        .duration(2500)

      path
        .attr('stroke-dashoffset', pathLength)
        .attr('stroke-dasharray', pathLength)
        .transition(transitionPath)
        .attr('stroke-dashoffset', 0)
    },
  },
}
</script>

<style scoped>
.path {
  stroke-dasharray: 1000;
  stroke-dashoffset: 1000;
  animation: dash 5s linear alternate infinite;
}

@keyframes dash {
  from {
    stroke-dashoffset: 1000;
  }
  to {
    stroke-dashoffset: 0;
  }
}
</style>
