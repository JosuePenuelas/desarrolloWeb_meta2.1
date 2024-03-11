<template>
  <v-card flat>
    <v-card-title>Usuarios</v-card-title>
    <template v-slot:text>
      <v-text-field v-model="search" label="Buscar" prepend-inner-icon="mdi-magnify" single-line variant="outlined"
        hide-details></v-text-field>
    </template>

    <v-container>
      <v-btn class="mb-2" color="primary" @click="showNewDialog = true">
        New Item
      </v-btn>
      <v-btn class="mb-2" color="secondary" @click="showUpdateDialog = true">
        Update Item
      </v-btn>
      <v-btn class="mb-2" color="red" @click="showDeleteDialog = true">
        Delete Item
      </v-btn>
    </v-container>

    <v-data-table :headers="headers" :items="datos" :search="search" :sort-by="[{ key: 'id', order: 'asc' }]"
      :sort-by1="[{ key: 'title', order: 'asc' }]" :sort-by2="[{ key: 'body', order: 'asc' }]"></v-data-table>

    <!-- Diálogo para introducir ID a eliminar -->
    <v-dialog v-model="showDeleteDialog" max-width="500px">
      <v-card>
        <v-card-title>Eliminar Elemento</v-card-title>
        <v-card-text>
          <v-text-field v-model.number="idToDelete" label="ID a eliminar" type="number"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="red darken-1" text @click="eliminar(idToDelete); showDeleteDialog = false">Eliminar</v-btn>
          <v-btn text @click="showDeleteDialog = false">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Diálogo para introducir nuevos datos -->
    <v-dialog v-model="showNewDialog" max-width="500px">
      <v-card>
        <v-card-title>Nuevo Elemento</v-card-title>
        <v-card-text>
          <v-text-field v-model="newTitle" label="Título"></v-text-field>
          <v-text-field v-model="newBody" label="Cuerpo"></v-text-field>
          <v-text-field v-model.number="newUserId" label="Usuario ID" type="number"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" @click="crearNuevoItem">Crear</v-btn>
          <v-btn text @click="showNewDialog = false">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Diálogo para actualizar datos -->
    <v-dialog v-model="showUpdateDialog" max-width="500px">
      <v-card>
        <v-card-title>Actualizar Elemento</v-card-title>
        <v-card-text>
          <v-text-field v-model.number="updateId" label="ID a actualizar" type="number"></v-text-field>
          <v-text-field v-model="updateTitle" label="Nuevo Título"></v-text-field>
          <v-text-field v-model="updateBody" label="Nuevo Cuerpo"></v-text-field>
          <v-text-field v-model.number="updateUserId" label="Nuevo Usuario ID" type="number"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" @click="actualizarElemento">Actualizar</v-btn>
          <v-btn text @click="showUpdateDialog = false">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

  </v-card>
</template>

<script setup>
import { ref } from 'vue';

const search = ref('');
const headers = ref([
  { align: 'start', key: 'userId', sortable: false, title: 'Usuario ID' },
  { key: 'id', title: 'ID' },
  { key: 'title', title: 'Título' },
  { key: 'body', title: 'Cuerpo' }
]);

const datos = ref([]);

//para borrar
const showDeleteDialog = ref(false);
const idToDelete = ref('');

//para nuevo dato
const showNewDialog = ref(false);
const newTitle = ref('');
const newBody = ref('');
const newUserId = ref('');

//para actualizar dato
const showUpdateDialog = ref(false);
const updateId = ref('');
const updateTitle = ref('');
const updateBody = ref('');
const updateUserId = ref('');


async function obtenerDatos() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");
    const data = await response.json();
    datos.value = data;
  } catch (error) {
    console.error('Error al obtener datos:', error);
  }
}

async function crearNuevoItem() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      body: JSON.stringify({
        title: newTitle.value,
        body: newBody.value,
        userId: newUserId.value,
      }),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    });

    if (response.ok) {
      const data = await response.json();
      console.log(data);
      datos.value.push(data);
      showNewDialog.value = false;
    } else {
      throw new Error('Hubo un problema al crear el nuevo post.');
    }

  } catch (error) {
    console.error('Error al crear el nuevo post:', error);
  }
}

async function actualizarElemento() {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${updateId.value}`, {
      method: 'PUT',
      body: JSON.stringify({
        id: updateId.value,
        title: updateTitle.value,
        body: updateBody.value,
        userId: updateUserId.value,
      }),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    });

    if (response.ok) {
      const updatedData = await response.json();
      console.log(updatedData);

      const index = datos.value.findIndex(post => post.id === updateId.value);
      if (index !== -1) {
        datos.value[index] = updatedData;
      }

      showUpdateDialog.value = false;
    } else {
      throw new Error('Hubo un problema al actualizar el post.');
    }

  } catch (error) {
    console.error('Error al actualizar el post:', error);
  }
}

async function eliminar(id) {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
      method: 'DELETE',
    });

    const data = await response.json();
    console.log(data);
    const index = datos.value.findIndex(post => post.id === id);
    if (index !== -1) {
      datos.value.splice(index, 1);
    }

  } catch (error) {
    console.error('Error al crear el delete:', error);
  }
}

obtenerDatos();
// crearPost();
// eliminar(1);
// update(5);
</script>
