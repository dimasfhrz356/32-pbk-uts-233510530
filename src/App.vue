<script setup>
import { ref, computed } from 'vue';

const listTugas = ref([]);
const newTugas = ref('');
const filterTugas = ref('semua');

const tambahTugas = () => {
  if (newTugas.value.trim() !== '') {
    listTugas.value.push(
      {
        id: Date.now(),
        text: newTugas.value,
        selesai: false,
      }
    )
    newTugas.value = '';
  }
}

const totalTugas = computed(() => listTugas.value.length);
const selesaiTugas = computed(() => listTugas.value.filter(tugas => tugas.selesai).length);
const tugasBelumSelesai = computed(() => listTugas.value.filter(tugas => !tugas.selesai).length);

const hapusTugas = (id) => {
  listTugas.value = listTugas.value.filter(tugas => tugas.id !== id);
}

const filterTugasList = computed(() => {
  if (filterTugas.value === 'selesai') {
    return listTugas.value.filter(tugas => tugas.selesai);
  } else if (filterTugas.value === 'belum-selesai') {
    return listTugas.value.filter(tugas => !tugas.selesai);
  } else {
    return listTugas.value;
  }
})

</script>

<template>
  <div class="flex min-h-screen bg-gradient-to-r from-sky-100 to-emerald-100 p-10">
    <div class="w-1/3 p-6 rounded-lg bg-sky-200 shadow-lg">
      <h1 class="text-2xl font-bold mb-4 text-sky-900">Tambah Tugas</h1>
      <input type="text" v-model="newTugas" @keyup.enter="tambahTugas" placeholder="Contoh: Belajar Vue"
        class="w-full p-2 mb-3 rounded border border-sky-300 focus:outline-none focus:ring-2 focus:ring-sky-500" />
      <button @click="tambahTugas" class="w-full bg-sky-600 text-white py-2 rounded hover:bg-sky-700 transition">
        Tambahkan
      </button>

      <div class="mt-6 space-x-2">
        <button @click="filterTugas = 'semua'" class="px-3 py-1 bg-white rounded shadow hover:bg-sky-100">Semua</button>
        <button @click="filterTugas = 'selesai'"
          class="px-3 py-1 bg-white rounded shadow hover:bg-sky-100">Selesai</button>
        <button @click="filterTugas = 'belum-selesai'" class="px-3 py-1 bg-white rounded shadow hover:bg-sky-100">Belum
          Selesai</button>
      </div>
    </div>

    <div class="w-2/3 p-6 ml-6 bg-white/60 rounded-lg shadow-lg backdrop-blur">
      <h2 class="text-xl font-semibold text-emerald-900 mb-4">Daftar Tugas</h2>
      <ul class="space-y-3 max-h-[400px] overflow-y-auto pr-2">
        <li v-for="tugas in filterTugasList" :key="tugas.id"
          class="flex justify-between items-center bg-white p-3 rounded-lg shadow">
          <div class="flex items-center gap-3">
            <input type="checkbox" v-model="tugas.selesai" class="accent-emerald-500" />
            <span :class="{ 'line-through text-gray-400': tugas.selesai }" class="text-lg">
              {{ tugas.text }}
            </span>
          </div>
          <button @click="hapusTugas(tugas.id)" class="text-red-500 hover:text-red-700 font-medium">Hapus</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
ul::-webkit-scrollbar {
  width: 6px;
}

ul::-webkit-scrollbar-thumb {
  background-color: #38bdf8;
  border-radius: 6px;
}

ul::-webkit-scrollbar-track {
  background: transparent;
}
</style>
