<template>
  <v-card flat title="Usuarios">
    <template v-slot:text>
      <v-text-field v-model="search" label="Search" prepend-inner-icon="mdi-magnify" single-line variant="outlined"
        hide-details></v-text-field>
    </template>

    <v-data-table :headers="headers" :items="datos" :search="search"></v-data-table>
  </v-card>
</template>
<script setup>
import { ref } from 'vue';
const search = ref('');
const headers = ref(
  [
    {
      align: 'start',
      key: 'userId',
      sortable: false,
      title: 'Usuario ID',
    },
    { key: 'id', title: 'ID' },
    { key: 'title', title: 'Titulo' },
    { key: 'completed', title: 'completado' },
  ]

);

const datos = ref('');

async function obtenerDatos() {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos");
  const data = await response.json();
  datos.value = data;
}

obtenerDatos();

</script>