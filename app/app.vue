<template>
  <v-app>
    <!-- Cabeçalho -->

    <v-app-bar color="primary" dark>
      <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-app-bar-title>Árvore Genealógica - Araripina</v-app-bar-title>


      <template v-if="$vuetify.display.mdAndUp">
        <v-btn icon="mdi-magnify" variant="text"></v-btn>

        <v-btn icon="mdi-filter" variant="text"></v-btn>
      </template>

      <v-btn icon="mdi-dots-vertical" variant="text"></v-btn>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" :location="$vuetify.display.mobile ? 'bottom' : undefined" temporary>
      <NuxtLink style="text-decoration:none; color: inherit;" to="/">
        <v-list-item>
        Inicio
      </v-list-item>
      </NuxtLink>
      <NuxtLink style="text-decoration:none; color: inherit;" to="/pessoas">
        <v-list-item>
        Pessoas
      </v-list-item>
      </NuxtLink>
    </v-navigation-drawer>


    <!-- Conteúdo principal -->
    <v-main class="pa-4">
      <NuxtPage /> <!-- Aqui as páginas serão injetadas -->
    </v-main>

    <!-- Rodapé -->
    <v-footer class="pa-4">
      <span>&copy; 2025 Cidade de Araripina</span>
    </v-footer>
  </v-app>
</template>
<script setup>
import { ref, watch } from 'vue'

const items = [
  {
    title: 'Foo',
    value: 'foo',
  },
  {
    title: 'Bar',
    value: 'bar',
  },
  {
    title: 'Fizz',
    value: 'fizz',
  },
  {
    title: 'Buzz',
    value: 'buzz',
  },
]

const drawer = ref(false)
const group = ref(null)

watch(group, () => {
  drawer.value = false
})
</script>