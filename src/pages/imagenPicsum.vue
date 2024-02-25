<script setup>
import { ref } from 'vue'

const urlImagen = ref("https://picsum.photos/870");
const joke = ref("");
const show = ref(false);

async function cambiaImagen() {
    let respuesta = await fetch("https://picsum.photos/870");
    console.log(respuesta.url)
    urlImagen.value = respuesta.url;
}

async function fetchChuckNorrisJoke() {
    const response = await fetch('https://api.chucknorris.io/jokes/random');
    const data = await response.json();
    joke.value = data.value;
}

fetchChuckNorrisJoke();

</script>

<template>
    <div class="mt-4 text-subtitle-2"> </div>
    <v-card class="mx-auto" max-width="500">
        <v-img :src="urlImagen" height="500px" cover></v-img>

        <v-card-title>
            Imagen de Picsum
        </v-card-title>

        <v-card-subtitle>

        </v-card-subtitle>

        <v-card-actions>
            <v-btn color="orange-lighten-2" variant="text" @click="cambiaImagen">
                Nueva Imagen
            </v-btn>

            <v-spacer></v-spacer>

            <v-btn :icon="show ? 'mdi-chevron-up' : 'mdi-chevron-down'" @click="show = !show"></v-btn>
        </v-card-actions>

        <v-expand-transition>
            <div v-show="show">
                <v-divider></v-divider>
                <v-card-title>
                    Chuck Norris Jokes
                </v-card-title>
                <v-card-text>
                    {{ joke }}
                </v-card-text>
            </div>
        </v-expand-transition>
    </v-card>
</template>