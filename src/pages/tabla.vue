<script setup>
import { ref } from 'vue'

const datos = ref("");

async function obtenerDatos() {
  const response = await fetch("https://jsonplaceholder.typicode.com/todos");
  const data = await response.json();
  datos.value = data.value;
}

obtenerDatos();
console.log(datos);
</script>

<template>
  <v-table theme="dark">
    <thead>
      <tr>
        <th class="text-left">
          ID usuario
        </th>
        <th class="text-left">
          ID
        </th>
        <th class="text-left">
          Titulo
        </th>
        <th class="text-left">
          Completado
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in datos" :key="item.userId">
        <td>{{ item.userId }}</td>
        <td>{{ item.id }}</td>
        <td>{{ item.title }}</td>
        <td>{{ item.completed }}</td>
      </tr>
    </tbody>
  </v-table>
</template>