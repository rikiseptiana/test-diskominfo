<template>
  <div class="chart-container">
    <h2>Jumlah Data per Kecamatan</h2>
    <canvas ref="chartRef"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { Chart, registerables } from 'chart.js'

// Daftarkan semua komponen Chart.js
Chart.register(...registerables)

const chartRef = ref(null)

onMounted(async () => {
  const response = await fetch('https://agusdev.sumedangkab.go.id/api/data')
  const json = await response.json()
  const data = json.data

  const countPerKecamatan = {}
  for (const item of data) {
    const kecamatan = item.kecamatan || 'Tidak diketahui'
    countPerKecamatan[kecamatan] = (countPerKecamatan[kecamatan] || 0) + 1
  }

  const labels = Object.keys(countPerKecamatan)
  const values = Object.values(countPerKecamatan)

  new Chart(chartRef.value, {
    type: 'bar',
    data: {
      labels,
      datasets: [{
        label: 'Jumlah Penduduk per Kecamatan',
        data: values,
        backgroundColor: 'rgba(75, 192, 192, 0.6)',
        borderColor: 'rgba(75, 192, 192, 1)',
        borderWidth: 1
      }]
    },
    options: {
      responsive: true,
      plugins: {
        legend: { display: true },
        title: {
          display: true,
          text: 'Visualisasi Kecamatan'
        }
      },
      scales: {
        y: { beginAtZero: true, ticks: { precision: 0 } }
      }
    }
  })
})
</script>

<style scoped>
.chart-container {
  max-width: 800px;
  margin: 40px auto;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 6px;
  background-color: #f9f9f9;
}
canvas {
  width: 100%;
  height: auto;
}
</style>
//   border: 1px solid #ddd;

<template>
  <div>
    <h1 style="text-align:center; margin:20px 0;">Visualisasi Data</h1>
    <DataChart />
  </div>
</template>

<script setup>
import DataChart from './components/DataChart.vue'
</script>
