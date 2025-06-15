<template>
  <div class="container">
    <h2>Tabel Data Penduduk</h2>

    <div v-if="loading">Memuat data...</div>
    <div v-else-if="error" class="error">{{ error }}</div>

    <table v-else>
      <thead>
        <tr>
          <th>#</th>
          <th>NIK</th>
          <th>Nama Lengkap</th>
          <th>Jenis Kelamin</th>
          <th>Tempat, Tanggal Lahir</th>
          <th>Agama</th>
          <th>Status Perkawinan</th>
          <th>Pekerjaan</th>
          <th>Pendidikan Terakhir</th>
          <th>Alamat</th>
          <th>Wilayah</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in records" :key="item.id">
          <td>{{ index + 1 }}</td>
          <td>{{ item.nik }}</td>
          <td>{{ item.nama_lengkap }}</td>
          <td>{{ item.jenis_kelamin }}</td>
          <td>{{ item.tempat_lahir }}, {{ item.tanggal_lahir }}</td>
          <td>{{ item.agama }}</td>
          <td>{{ item.status_perkawinan }}</td>
          <td>{{ item.pekerjaan }}</td>
          <td>{{ item.pendidikan_terakhir }}</td>
          <td>
            {{ item.alamat }}<br />
            RT {{ item.rt }}/RW {{ item.rw }} - Dusun {{ item.dusun }}
          </td>
          <td>
            Desa {{ item.desa }}<br />
            Kec. {{ item.kecamatan }}<br />
            Kab. {{ item.kabupaten }}<br />
            Prov. {{ item.provinsi }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const records = ref([])
const loading = ref(true)
const error = ref(null)

onMounted(async () => {
  try {
    const response = await fetch('https://agusdev.sumedangkab.go.id/api/data')
    const json = await response.json()
    console.log('Response:', json)

    const data = json.data
    if (!Array.isArray(data)) {
      throw new Error('Format data tidak sesuai (data harus array)')
    }

    records.value = data
  } catch (err) {
    error.value = 'Gagal mengambil data: ' + err.message
  } finally {
    loading.value = false
  }
})
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 40px auto;
  padding: 20px;
  background: #f9f9f9;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-family: Arial, sans-serif;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
}

.error {
  color: red;
  font-weight: bold;
  margin-top: 10px;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
  margin-top: 10px;
}

th, td {
  border: 1px solid #999;
  padding: 8px;
  text-align: left;
  vertical-align: top;
}

th {
  background-color: #eee;
}

tr:nth-child(even) {
  background-color: #f3f3f3;
}

tr:hover {
  background-color: #e6f7ff;
}
</style>
