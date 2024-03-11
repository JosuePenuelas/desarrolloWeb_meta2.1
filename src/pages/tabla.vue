<template>
  <v-card flat>
    <v-card-title>Usuarios</v-card-title>
    <template v-slot:text>
      <v-text-field v-model="search" label="Buscar" prepend-inner-icon="mdi-magnify" single-line variant="outlined" hide-details></v-text-field>
    </template>

    <v-data-table :headers="headers" :items="datos" :search="search"></v-data-table>
  </v-card>
</template>

<script setup>
import { ref } from 'vue';

const search = ref('');
const headers = ref([
  { align: 'start', key: 'userId', sortable: false, title: 'Usuario ID' },
  { key: 'id', title: 'ID' },
  { key: 'title', title: 'Título' },
  { key: 'completed', title: 'Completado' }
]);

const datos = ref([]);

async function obtenerDatos() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    datos.value = data;
  } catch (error) {
    console.error('Error al obtener datos:', error);
  }
}

obtenerDatos();
</script>
