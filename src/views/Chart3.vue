<template>
  <div>
    <Line
      :chart-data="data"
    >
    </Line>
  </div>
</template>

<script>
import { Line } from 'vue-chartjs'
import { Chart as ChartJS, registerables } from 'chart.js'

ChartJS.register(...registerables)

export default {
  name: 'Chart3',
  components: { Line },
  setup() {
    const data = {
      datasets: [{
        label: 'line Dataset',
        data: [10, 20, 30, 40, 70, 90, 40],
        backgroundColor: 'yellow',
        order: 1,
      }],
      labels: ['월요일', '화요일', '수요일', '목요일', '금요일', '토요일', '일요일'],
      borderColor: 'white',
    }

    return {
      data,
    }
  },
}
</script>

<style scoped>

</style>
