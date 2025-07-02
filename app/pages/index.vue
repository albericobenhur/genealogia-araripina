<template>
  <section class="pt-8">
    <h1>Árvore Genealógica de Araripina</h1>
    <NuxtLink to="/sobre">Ir para Sobre</NuxtLink>
    <p>Bem-vindo ao sistema público de genealogia da cidade de Araripina. Aqui você pode pesquisar pessoas e famílias e descobrir graus de parentesco.</p>
    <div>
    
      <v-text-field v-model="searchTerm" @keydown.enter="navigateTo('/pessoas?q=' + searchTerm)" id="search" label="Nome ou sobrenome">
      </v-text-field>
    </div>
  </section>

</template>
<script setup lang="ts">

const searchTerm = ref('')
</script>