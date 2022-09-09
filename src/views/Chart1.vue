<template>
  <div class="bar">
    <Bar
      :chart-options="barChartOptions"
      :chart-data="barChartData"
      :chart-id="chartId"
      :dataset-id-key="datasetIdKey"
      :plugins="plugins"
      :css-classes="cssClasses"
      :styles="styles"
      :width="width"
      :height="height"
    />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs/legacy'
import
{
  Chart as ChartJS,
  registerables,
} from 'chart.js'
import { ref } from '@vue/composition-api'

ChartJS.register(...registerables)
export default {
  name: 'Chart1',
  components: { Bar },
  props: {
    chartId: {
      type: String,
      default: 'bar-chart',
    },
    datasetIdKey: {
      type: String,
      default: 'label',
    },
    width: {
      type: Number,
      default: 400,
    },
    height: {
      type: Number,
      default: 400,
    },
    cssClasses: {
      default: '',
      type: String,
    },
    styles: {
      type: Object,
      default: () => {},
    },
    plugins: {
      type: Object,
      default: () => {},
    },
  },
  setup() {
    const barChartOptions = {
      responsive: true,
      scales: {
        x: {
          stacked: true,
          display: true,
        },
        y: {
          stacked: true,
          display: true,
        },
      },
    }
    const barChartData = ref({
      labels: ['7/12', '8/12', '9/12', '10/12', '11/12', '12/12', '13/12', '14/12', '15/12', '16/12', '17/12'],
      datasets: [
        {
          label: 'Dataset 1',
          data: [275, 90, 190, 205, 125, 85, 55, 87, 127, 150, 230, 280, 190],
          backgroundColor: '#ffcf5c',
          borderColor: 'transparent',
          barThickness: 15,
        },
        {
          label: 'Dataset 2',
          data: [275, 90, 190, 205, 125, 85, 55, 87, 127, 150, 230, 280, 190],
          backgroundColor: '#faaf9c',
          borderColor: 'transparent',
          barThickness: 15,
        },
      ],
    })

    return {
      barChartOptions,
      barChartData,
    }
  },
}
</script>

<style scoped>
.bar {
  width: 400px;
}
</style>
