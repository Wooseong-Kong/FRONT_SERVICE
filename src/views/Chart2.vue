<template>
  <div class="heat">
    <vue-apex-charts
      id="heatChart"
      type="heatmap"
      width="100%"
      :height="chartHeight"
      :options="chartOptionsComputed"
      :series="data"
    />
  </div>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'
import { computed } from '@vue/composition-api'

export default {
  name: 'Chart2',
  components: { VueApexCharts },
  props: {
    data: {
      type: [Array, Object],
      default: () => [],
    },
    options: {
      type: Object,
      default: null,
    },
    chartHeight: {
      type: String,
      default: 'auto',
    },
  },
  setup(props) {
    const chartOptions = {
      dataLabels: {
        enabled: false,
      },
      legend: {
        show: true,
        position: 'bottom',
      },
      plotOptions: {
        heatmap: {
          radius: 0,
          colorScale: {
            ranges: [
              {
                from: 0,
                to: 10,
                name: '0-10',
                color: '#ffe6e6',
              },
              {
                from: 11,
                to: 20,
                name: '10-20',
                color: '#ffcccc',
              },
              {
                from: 21,
                to: 30,
                name: '20-30',
                color: '#ffb3b3',
              },
              {
                from: 31,
                to: 40,
                name: '30-40',
                color: '#ff9999',
              },
              {
                from: 41,
                to: 50,
                name: '40-50',
                color: '#ff8080',
              },
              {
                from: 51,
                to: 60,
                name: '50-60',
                color: '#ff6666',
              },
              {
                from: 61,
                to: 70,
                name: '60-70',
                color: '#ff4d4d',
              },
              {
                from: 71,
                to: 80,
                name: '70-80',
                color: '#ff3333',
              },
              {
                from: 81,
                to: 90,
                name: '80-90',
                color: '#ff1a1a',
              },
              {
                from: 91,
                to: 100,
                name: '90-100',
                color: '#ff0000',
              },
            ],
          },
        },
      },
      xaxis: {
        categories: ['00:00', '01:00', '02:00', '03:00', '04:00', '05:00', '06:00',
          '07:00', '08:00', '09:00', '10:00', '11:00', '12:00', '13:00', '14:00',
          '15:00', '16:00', '17:00', '18:00', '19:00', '20:00', '21:00', '22:00', '23:00'],
        labels: {
          show: true,
        },
        axisBorder: {
          show: false,
        },
        axisTicks: {
          show: false,
        },
      },
      chart: {
        height: 350,
        type: 'heatmap',
        toolbar: {
          show: false,
        },
      },
    }
    const chartOptionsComputed = computed(() => {
      if (props.options === null) {
        const options = JSON.parse(JSON.stringify(chartOptions))

        return options
      }

      return props.options
    })

    return {
      chartOptionsComputed,
    }
  },
}

</script>

<style lang="scss">
.heat{
  width: 800px;
}
#heatChart {
  .apexcharts-heatmap-rect {
    height: 30px;
    stroke-width: 0;
  }
  .apexcharts-yaxis {
    transform:translate(47px, -25px);
  }
  .apexcharts-grid {
    visibility: hidden;
  }
  .apexcharts-xaxis {
    transform: translate(0px, -50px);
  }
}
</style>
