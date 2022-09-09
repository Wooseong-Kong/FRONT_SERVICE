<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div class="bar">
      <Bar
        :chart-options="chartOptions"
        :chart-data="chartData"
        :chart-id="chartId"
        :dataset-id-key="datasetIdKey"
        :plugins="plugins"
        :css-classes="cssClasses"
        :styles="styles"
        :width="width"
        :height="height"
      />
    </div>

    <chart1>
    </chart1>

    <v-card-text>
      <chart2
        :key="chartRender"
        type="heatmap"
        chart-height="350"
        :data="inputHeatMapChartData"
      />
    </v-card-text>
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs/legacy'
import
{
  Chart as ChartJS,
  registerables,
} from 'chart.js'
import { mdiPencilOutline } from '@mdi/js'
import Chart1 from '@/views/Chart1.vue'
import Chart2 from '@/views/Chart2.vue'
import { ref, onMounted } from '@vue/composition-api'

ChartJS.register(...registerables)

export default {
  name: 'About',
  components: { Chart2, Chart1, Bar },
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
    const inputHeatMapChartData = ref([])

    const heatMapChartData = [
      {
        name: '연료전지1',
        data: [0, 0, 0, 0, 60, 0, 50, 0, 0, 0, 90, 80, 70, 10, 0, 50, 0, 0, 0, 40, 0, 0, 0, 0],
      },
      {
        name: '연료전지2',
        data: [0, 0, 0, 0, 60, 0, 50, 0, 0, 0, 30, 0, 0, 10, 70, 0, 50, 0, 80, 40, 0, 0, 20, 30],
      },
      {
        name: '연료전지3',
        data: [0, 0, 0, 0, 60, 0, 50, 0, 40, 0, 30, 0, 0, 10, 0, 70, 80, 90, 60, 40, 0, 0, 60, 70],
      },
    ]
    const chartRender = ref(0)
    const changeHeatMapChartData = () => {
      inputHeatMapChartData.value = heatMapChartData
      chartRender.value += 1
    }

    const chartOptions = { responsive: true }
    const chartData = {
      datasets: [{
        label: 'Bar Dataset',
        data: [10, 20, 30, 40, 70, 90, 40],
        backgroundColor: 'yellow',
        order: 1,
      }, {
        label: 'Line Dataset',
        data: [100, 130, 110, 120, 150, 160, 130],
        type: 'line',
        borderColor: 'red',
        backgroundColor: 'blue',
        fill: false,
        order: 2,
      }],
      labels: ['월요일', '화요일', '수요일', '목요일', '금요일', '토요일', '일요일'],
      borderColor: 'white',
    }

    onMounted(() => {
      changeHeatMapChartData()
    })

    return {
      chartRender,
      inputHeatMapChartData,
      chartOptions,
      chartData,
      icons: {
        mdiPencilOutline,
      },
    }
  },
}
</script>
<style lang="scss">
.bar {
  width: 400px;
}
</style>
