<script setup lang="ts">
import {
  Bar
} from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

defineProps < {
  chartId: {
    type: String,
    default: 'line-chart'
  },
  datasetIdKey: {
    type: String,
    default: 'label'
  },
  width: {
    type: Number,
    default: 400
  },
  height: {
    type: Number,
    default: 400
  },
  cssClasses: {
    default: '',
    type: String
  },
  styles: {
    type: Object,
    default: () => {}
  },
  plugins: {
    type: Object,
    default: () => {
      title: {
        display: true,
        text: 'Custom Chart Title',
        padding: {
          top: 10,
          bottom: 30
        }
      },
      subtitle: {
        display: true,
        text: 'Custom Chart Subtitle'
      }
    }
  }

} > ();



//ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

// export default {
//   name: 'BarChart',
//   components: { Bar },
//   props: {
//
//   };

const chartId = 'line-chart';
const chartData = {
  labels: ['January', 'February', 'March', 'april'],
  datasets: [{
    data: [40, 20, 12, 15],
    backgroundColor: [
      'rgb(255, 99, 132)',
      'rgb(54, 162, 235)',
      'rgb(255, 205, 86)'
    ]
  }]
}
const chartOptions = {
  responsive: true,
  maintainAspectRatio: false
}
</script>

<template>
<div class="row">
  <Bar :chart-options="chartOptions" :chart-data="chartData" :chart-id="chartId" :dataset-id-key="datasetIdKey" :plugins="plugins" :css-classes="cssClasses" :styles="styles" :width="width" :height="height" />
</div>
</template>
