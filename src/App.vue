<template>
  <div class="analytics-container">
    <div v-if="analyticsData">
      <h2>Idle Seasons Analytics</h2>
      <div class="season" v-for="season in analyticsData" :key="season.seasonOrder">
        <h3>Season {{ season.seasonOrder }}</h3>
        <div class="analytics-details">
          <p><strong>Total Public Addresses:</strong> {{ season.analytics.trackSession.totalPublicAddress }}</p>
          <p><strong>Total Sessions:</strong> {{ season.analytics.trackSession.totalSessions }}</p>
          <h4>VIP Track</h4>
          <ul>
            <li v-for="vip in season.analytics.trackVip" :key="vip._id">
              <strong>{{ vip._id }}:</strong> <strong>PVU Amount:</strong> {{ vip.pvuAmount }}, <strong>Buy Count:</strong> {{ vip.buyCount }}
            </li>
          </ul>
          <p><strong>Total Reach 1M:</strong> {{ season.analytics.reach1M.total }}</p>
          <p><strong>Average Time in Seconds:</strong> {{ season.analytics.reach1M.avgTimeInSecond }}</p>
        </div>
      </div>
    </div>
    <div v-else class="loading">Loading...</div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  name: 'pvu-analytics',
  setup() {
    const analyticsData = ref(null)

    const fetchData = async () => {
      try {
        const response = await fetch(
          'https://api.plantvsundead.com/mini-farms/analytics'
        )
        const data = await response.json()
        analyticsData.value = data.data.sort((a, b) => b.seasonOrder - a.seasonOrder); // Assuming the API returns an array of seasons
      } catch (error) {
        console.error('Error fetching analytics data:', error)
      }
    }

    onMounted(() => {
      fetchData()
    })

    return { analyticsData }
  },
}
</script>

<style scoped>
.analytics-container {
  font-family: Arial, sans-serif;
  padding: 20px;
}

.season {
  background-color: #f0f0f0;
  border-radius: 8px;
  margin-bottom: 20px;
  padding: 15px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.analytics-details {
  margin-top: 10px;
}

.loading {
  text-align: center;
}
</style>