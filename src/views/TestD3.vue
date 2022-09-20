<template>
  <div id="app">
    <v-card>
      <line-chart
      ></line-chart>
    </v-card>

    <div class="buttons">
      <button @click="addData">Add data</button>
      <button @click="filterData">Filter data</button>
    </div>

    <v-card>
      <line-chart2></line-chart2>
    </v-card>

  </div>
</template>

<script>
import LineChart from '@/views/D3Chart/LineChart.vue'
import LineChart2 from '@/views/D3Chart/LineChart2.vue'

export default {
  name: 'TestD3',
  components: { LineChart2, LineChart },
  data() {
    return {
      data: [10, 40, 15, 25, 50],
    }
  },
  methods: {
    addData() {
      // add random value from 0 to 50 to array
      this.data = [...this.data, Math.round(Math.random() * 50)]
    },
    filterData() {
      this.data = this.data.filter((v) => v <= 35)
    },
  },
}
</script>

<style scoped>
/*#app {*/
/*  font-family: Avenir, Helvetica, Arial, sans-serif;*/
/*  -webkit-font-smoothing: antialiased;*/
/*  -moz-osx-font-smoothing: grayscale;*/
/*  text-align: center;*/
/*  color: blue;*/
/*  max-width: 720px;*/
/*  margin: 100px auto;*/
/*  padding: 0 20px;*/
/*}*/
/*svg {*/
/*   !* important for responsiveness *!*/
/*   display: block;*/
/*   fill: none;*/
/*   stroke: none;*/
/*   width: 100%;*/
/*   height: 100%;*/
/*   overflow: visible;*/
/*   background: #eee;*/
/* }*/
.buttons {
  margin-top: 2rem;
}
</style>
