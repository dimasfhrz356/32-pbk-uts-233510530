<script setup>
import { ref } from 'vue';

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

</script>

<template>
  <div>
    <h1>To Do List</h1>
    <input type="text" v-model="newTugas" @keyup.enter="tambahTugas" placeholder="Tambah tugas">
    <button @click="tambahTugas">Tambahkan</button>
  </div>
</template>

<style scoped></style>
