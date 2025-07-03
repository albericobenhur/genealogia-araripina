<template>
  <v-main class="pa-0">
    <v-container>
      <header class="mb-6">
        <v-row>
          <v-col>
            <h1 class="text-h3 font-weight-bold">Famílias</h1>
            <p class="text-subtitle-1 text-medium-emphasis">
              Explore as famílias cadastradas em nossa genealogia
            </p>
          </v-col>
        </v-row>
      </header>

      <section>
        <v-row>
          <v-col>
            <v-card elevation="2">
              <v-card-title class="text-h5">
                Lista de Famílias
              </v-card-title>
              
              <v-list lines="two">
                <template v-for="(familia, index) in familias" :key="familia.sobrenome">
                  <v-list-item
                    @click="verDetalhes(familia)"
                    class="cursor-pointer"
                  >
                    <template #prepend>
                      <v-avatar color="primary">
                        <v-icon>mdi-account-group</v-icon>
                      </v-avatar>
                    </template>

                    <v-list-item-title class="text-h6">
                      {{ familia.sobrenome }}
                    </v-list-item-title>
                    
                    <v-list-item-subtitle>
                      {{ familia.membros }} {{ familia.membros === 1 ? 'membro' : 'membros' }}
                    </v-list-item-subtitle>

                    <template #append>
                      <v-btn 
                        icon="mdi-chevron-right" 
                        variant="text" 
                        size="small"
                        :aria-label="`Ver detalhes da família ${familia.sobrenome}`"
                        @click.stop="verDetalhes(familia)"
                      />
                    </template>
                  </v-list-item>

                  <v-divider 
                    v-if="index < familias.length - 1" 
                    :key="`divider-${familia.sobrenome}`"
                  />
                </template>
              </v-list>
            </v-card>
          </v-col>
        </v-row>
      </section>
    </v-container>
  </v-main>
</template>

<script setup>
// Dados das famílias
const familias = [
  { sobrenome: 'Silva', membros: 42 },
  { sobrenome: 'Souza', membros: 37 },
  { sobrenome: 'Lima', membros: 29 }
]

// Função para navegar para detalhes da família
const verDetalhes = (familia) => {
  navigateTo(`/familias/${familia.sobrenome.toLowerCase()}`)
}
</script>