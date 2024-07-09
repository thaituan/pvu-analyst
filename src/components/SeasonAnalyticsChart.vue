<template>
  <div class="chart-container">
    <BarChart :chart-data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { ref } from 'vue'

export default {
  name: 'SeasonAnalyticsChart',
  components: {
    BarChart: Bar,
  },
  props: {
    seasonData: {
      type: Object,
      required: true,
    },
  },
  setup(props) {
    const chartData = ref({
      labels: props.seasonData.analytics.trackVip.map((vip) => vip._id),
      datasets: [
        {
          label: 'PVU Amount',
          backgroundColor: '#42A5F5',
          data: props.seasonData.analytics.trackVip.map((vip) => vip.pvuAmount),
        },
        {
          label: 'Buy Count',
          backgroundColor: '#66BB6A',
          data: props.seasonData.analytics.trackVip.map((vip) => vip.buyCount),
        },
      ],
    })

    const chartOptions = ref({
      responsive: true,
      maintainAspectRatio: false,
    })

    return { chartData, chartOptions }
  },
}
</script>

<style scoped>
.chart-container {
  height: 400px;
  width: 600px;
}
</style>
