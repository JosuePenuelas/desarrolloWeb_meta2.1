<template>
  <v-card>
      <v-layout>
          <v-navigation-drawer expand-on-hover rail color="indigo">
              <v-list>
                  <v-list-item prepend-icon="mdi-vuetify" title="Primera App" subtitle="con vuetify"></v-list-item>
              </v-list>

              <v-divider></v-divider>

              <v-list density="compact" nav>
                <v-list-item prepend-icon="mdi-home" link to="/" title="Inicio" value="Inicio"></v-list-item>
                  <v-list-item prepend-icon="mdi-image-multiple" link to="/ImagenPicsum" title="Imagenes" value="myfiles"></v-list-item>
                  <v-list-item prepend-icon="mdi-text-box-outline" link to="/tabla" title="Datos" value="starred"></v-list-item>
              </v-list>
          </v-navigation-drawer>

          <v-main style="height: auto; width: auto;">
              <RouterView />
          </v-main>
      </v-layout>
  </v-card>
</template>

<script setup>
//
</script>
