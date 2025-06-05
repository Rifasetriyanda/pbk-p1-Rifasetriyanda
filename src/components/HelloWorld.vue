<script setup>
import { ref } from 'vue'

const nama = ref('')
const tanggal = ref('')
const jam = ref('')
const durasi = ref(1)
const daftarBooking = ref([])
const pesan = ref('')
const error = ref('')

const tambahBooking = () => {
  if (!nama.value || !tanggal.value || !jam.value || durasi.value <= 0) {
    error.value = '❌ Semua kolom wajib diisi!'
    pesan.value = ''
    return
  }

  daftarBooking.value.push({
    nama: nama.value,
    tanggal: tanggal.value,
    jam: jam.value,
    durasi: durasi.value
  })

  pesan.value = `✅ Booking berhasil untuk ${nama.value} pada ${tanggal.value} jam ${jam.value} (Durasi ${durasi.value} jam)`
  error.value = ''

  // reset form
  nama.value = ''
  tanggal.value = ''
  jam.value = ''
  durasi.value = 1

  setTimeout(() => {
    pesan.value = ''
  }, 5000)
}
</script>

<template>
  <div class="wrapper">
    <h1 class="title">🎧 Booking Studio Musik</h1>

    <form @submit.prevent="tambahBooking" class="booking-form">
      <label>🎤 Nama Pemesan</label>
      <input v-model="nama" type="text" placeholder="Contoh: Fhelia Band" />

      <label>📅 Tanggal Booking</label>
      <input v-model="tanggal" type="date" />

      <label>⏰ Jam Mulai</label>
      <input v-model="jam" type="time" />

      <label>⏳ Durasi (jam)</label>
      <input v-model="durasi" type="number" min="1" />

      <button :disabled="!nama || !tanggal || !jam || durasi <= 0">🎶 Booking Sekarang</button>

      <transition name="fade">
        <p class="error" v-if="error">{{ error }}</p>
      </transition>
      <transition name="fade">
        <p class="success" v-if="pesan">{{ pesan }}</p>
      </transition>
    </form>

    <div class="jadwal" v-if="daftarBooking.length">
      <h2>📋 Daftar Booking</h2>
      <ul>
        <li v-for="(item, i) in daftarBooking" :key="i">
          <strong>{{ item.nama }}</strong> – {{ item.tanggal }} jam {{ item.jam }} (Durasi: {{ item.durasi }} jam)
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  max-width: 650px;
  margin: 40px auto;
  padding: 20px;
  font-family: 'Segoe UI', sans-serif;
  color: #333;
}

.title {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

.booking-form {
  background: #fefefe;
  padding: 20px;
  border: 2px solid #e3e3e3;
  border-radius: 12px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.05);
}

.booking-form label {
  display: block;
  margin-top: 12px;
  font-weight: bold;
}

.booking-form input {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border-radius: 8px;
  border: 1px solid #ccc;
  font-size: 14px;
}

.booking-form button {
  margin-top: 20px;
  background: linear-gradient(to right, #3498db, #2980b9);
  color: white;
  padding: 12px;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  width: 100%;
  transition: background 0.3s;
}

.booking-form button:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.success, .error {
  margin-top: 15px;
  padding: 10px;
  border-radius: 8px;
  text-align: center;
  font-weight: bold;
}

.success {
  background-color: #d4edda;
  color: #155724;
}

.error {
  background-color: #f8d7da;
  color: #721c24;
}

.jadwal {
  margin-top: 30px;
  padding: 20px;
  background-color: #f0f6ff;
  border-left: 4px solid #3498db;
  border-radius: 8px;
}

.jadwal ul {
  padding-left: 20px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>