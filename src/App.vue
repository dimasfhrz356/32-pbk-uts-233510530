<script setup>
import { ref, computed } from 'vue';

const listTugas = ref([]);
const newTugas = ref('');

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

</script>

<template>
  <div>
    <h1>To Do List</h1>
    <input type="text" v-model="newTugas" @keyup.enter="tambahTugas" placeholder="Tambah tugas">
    <button @click="tambahTugas">Tambahkan</button>

    <ul>
      <li v-for="tugas in listTugas" :key="tugas.id">
        <input type="checkbox" v-model="tugas.selesai">
        {{ tugas.text }}
        <button @click="hapusTugas(tugas.id)">Hapus</button>
      </li>
    </ul>

    <div>
      <p>
        total : {{ totalTugas }}
      </p>
      <p>
        Selesai : {{ selesaiTugas }}
      </p>
      <p>
        Belum Selesai : {{ tugasBelumSelesai }}
      </p>
    </div>
  </div>
</template>

<style scoped></style>
