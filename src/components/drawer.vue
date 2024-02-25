<template>
    <v-card>
        <v-layout>
            <v-navigation-drawer expand-on-hover rail>
                <v-list>
                    <v-list-item prepend-icon="mdi-vuetify" title="Primera App" subtitle="con vuetify"></v-list-item>
                </v-list>

                <v-divider></v-divider>

                <v-list density="compact" nav>
                    <v-list-item prepend-icon="mdi-image-multiple" link to="/ImagenPicsum" title="Imagenes" value="myfiles"></v-list-item>
                    <v-list-item prepend-icon="mdi-emoticon-lol-outline" title="Chistes" value="shared"></v-list-item>
                    <v-list-item prepend-icon="mdi-text-box-outline" title="Datos" value="starred"></v-list-item>
                </v-list>
            </v-navigation-drawer>

            <v-main style="height: 1000px">
                <HelloWorld />
            </v-main>
        </v-layout>
    </v-card>
</template>

<script setup>
import HelloWorld from '@/components/HelloWorld.vue';
//
</script>