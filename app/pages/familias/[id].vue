<template>
  <!-- Hero Banner -->
  <v-banner 
    class="bg-gradient-to-r from-primary to-secondary text-white" 
    height="300"
  >
    <v-container>
      <v-row align="center" class="fill-height">
        <v-col cols="12">
          <v-breadcrumbs
            :items="breadcrumbItems"
            divider="/"
            class="text-white mb-4"
          >
            <template #item="{ item }">
              <v-breadcrumbs-item
                :to="item.to"
                :disabled="item.disabled"
                class="text-white"
              >
                {{ item.title }}
              </v-breadcrumbs-item>
            </template>
          </v-breadcrumbs>
          
          <h1 class="text-h3 font-weight-bold mb-2">
            Família {{ familyData.name }}
          </h1>
          <p class="text-h6 font-weight-light">
            {{ familyData.members.length }} {{ familyData.members.length === 1 ? 'membro registrado' : 'membros registrados' }}
          </p>
        </v-col>
      </v-row>
    </v-container>
  </v-banner>

  <!-- Informações da Família -->
  <v-container class="py-8">
    <v-row>
      <v-col cols="12" md="8">
        <v-card elevation="2" rounded="lg" class="mb-6">
          <v-card-title class="text-h5 text-primary">
            <v-icon class="mr-2">mdi-information</v-icon>
            Informações Gerais
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col cols="12" sm="6">
                <v-list lines="two">
                  <v-list-item>
                    <template #prepend>
                      <v-icon color="primary">mdi-map-marker</v-icon>
                    </template>
                    <v-list-item-title>Origem</v-list-item-title>
                    <v-list-item-subtitle>{{ familyData.origin }}</v-list-item-subtitle>
                  </v-list-item>
                  
                  <v-list-item>
                    <template #prepend>
                      <v-icon color="primary">mdi-account</v-icon>
                    </template>
                    <v-list-item-title>Fundador</v-list-item-title>
                    <v-list-item-subtitle>{{ familyData.founder }}</v-list-item-subtitle>
                  </v-list-item>
                  
                  <v-list-item>
                    <template #prepend>
                      <v-icon color="primary">mdi-calendar</v-icon>
                    </template>
                    <v-list-item-title>Ano de Chegada</v-list-item-title>
                    <v-list-item-subtitle>{{ familyData.arrivalYear }}</v-list-item-subtitle>
                  </v-list-item>
                </v-list>
              </v-col>
              
              <v-col cols="12" sm="6" class="d-flex justify-center">
                <v-img
                  :src="familyData.image"
                  :alt="`Foto da família ${familyData.name}`"
                  height="200"
                  width="200"
                  cover
                  rounded="lg"
                  class="elevation-4"
                >
                  <template #placeholder>
                    <div class="d-flex align-center justify-center fill-height">
                      <v-icon size="64" color="grey-lighten-2">mdi-family-tree</v-icon>
                    </div>
                  </template>
                </v-img>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>

        <!-- História da Família -->
        <v-card elevation="2" rounded="lg">
          <v-card-title class="text-h5 text-primary">
            <v-icon class="mr-2">mdi-book-open</v-icon>
            História da Família
          </v-card-title>
          <v-card-text>
            <p class="text-body-1 line-height-lg">
              {{ familyData.history }}
            </p>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Sidebar com ações -->
      <v-col cols="12" md="4">
        <v-card elevation="2" rounded="lg" class="mb-6">
          <v-card-title class="text-h6 text-primary">
            Ações Rápidas
          </v-card-title>
          <v-card-text class="pa-2">
            <v-list nav>
              <v-list-item
                prepend-icon="mdi-plus"
                title="Adicionar Membro"
                @click="adicionarMembro"
                rounded="lg"
                class="mb-1"
              />
              <v-list-item
                prepend-icon="mdi-pencil"
                title="Editar Família"
                @click="editarFamilia"
                rounded="lg"
                class="mb-1"
              />
              <v-list-item
                prepend-icon="mdi-family-tree"
                title="Ver Árvore"
                @click="verArvore"
                rounded="lg"
              />
            </v-list>
          </v-card-text>
        </v-card>

        <!-- Estatísticas -->
        <v-card elevation="2" rounded="lg">
          <v-card-title class="text-h6 text-primary">
            Estatísticas
          </v-card-title>
          <v-card-text>
            <v-row class="text-center">
              <v-col cols="6">
                <div class="text-h4 text-primary font-weight-bold">
                  {{ familyData.members.length }}
                </div>
                <div class="text-caption text-medium-emphasis">
                  Membros
                </div>
              </v-col>
              <v-col cols="6">
                <div class="text-h4 text-secondary font-weight-bold">
                  {{ calcularGeracoes() }}
                </div>
                <div class="text-caption text-medium-emphasis">
                  Gerações
                </div>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>

  <!-- Membros da Família -->
  <v-container class="py-0">
    <v-card elevation="2" rounded="lg">
      <v-card-title class="text-h5 text-primary">
        <v-icon class="mr-2">mdi-account-group</v-icon>
        Membros da Família
      </v-card-title>
      
      <v-card-text v-if="loading" class="text-center py-8">
        <v-progress-circular indeterminate color="primary" size="64" />
        <p class="mt-4 text-medium-emphasis">Carregando membros...</p>
      </v-card-text>
      
      <v-card-text v-else>
        <v-row>
          <v-col 
            v-for="membro in familyData.members" 
            :key="membro.id"
            cols="12" 
            sm="6" 
            md="4"
          >
            <v-card 
              variant="outlined" 
              class="h-100 hover-lift cursor-pointer"
              @click="verMembro(membro.id)"
            >
              <v-card-item class="text-center pa-4">
                <v-avatar size="64" color="primary" class="mb-3">
                  <v-icon size="32">mdi-account</v-icon>
                </v-avatar>
                
                <v-card-title class="text-h6 mb-1">
                  {{ membro.name }}
                </v-card-title>
                
                <v-card-subtitle class="text-caption mb-2">
                  {{ membro.relationship }}
                </v-card-subtitle>
                
                <v-chip 
                  size="small" 
                  color="primary" 
                  variant="outlined"
                  class="text-caption"
                >
                  {{ formatarData(membro.birthDate) }}
                </v-chip>
              </v-card-item>
            </v-card>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script setup lang="ts">
// Meta e SEO
useHead({
  title: 'Detalhes da Família - Genealogia de Araripina',
  meta: [
    { name: 'description', content: 'Informações detalhadas sobre a família, membros e história genealógica.' },
    { property: 'og:title', content: 'Família - Genealogia de Araripina' },
    { property: 'og:description', content: 'Explore a história e membros desta família de Araripina' }
  ]
})

// Interfaces
interface Membro {
  id: number
  name: string
  birthDate: string
  relationship: string
}

interface FamilyData {
  name: string
  origin: string
  founder: string
  arrivalYear: string
  image: string
  history: string
  members: Membro[]
}

// Reactive state
const route = useRoute()
const familyId = route.params.id as string
const loading = ref(true)

// Breadcrumbs
const breadcrumbItems = ref([
  { title: 'Início', to: '/' },
  { title: 'Famílias', to: '/familias' },
  { title: 'Detalhes', disabled: true }
])

// Dados da família (simulação - substituir por API real)
const familyData = ref<FamilyData>({
  name: '',
  origin: '',
  founder: '',
  arrivalYear: '',
  image: '',
  history: '',
  members: []
})

// Função para buscar dados da família
const buscarDadosFamilia = async (): Promise<void> => {
  try {
    loading.value = true
    
    // Simulação de chamada API - substituir por chamada real
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // Dados mockados baseados no ID
    const dadosMock: Record<string, FamilyData> = {
      silva: {
        name: 'Silva',
        origin: 'Araripina, PE',
        founder: 'João Silva',
        arrivalYear: '1850',
        image: '/placeholder-family.jpg',
        history: 'A família Silva chegou à região de Araripina em meados do século XIX, vindos do interior da Paraíba. João Silva, o patriarca, estabeleceu-se na região como comerciante e criador de gado, contribuindo significativamente para o desenvolvimento local. Ao longo das gerações, a família manteve tradições rurais e se envolveu em atividades comerciais e políticas da cidade.',
        members: [
          { id: 1, name: 'João Silva', birthDate: '1825-03-15', relationship: 'Fundador' },
          { id: 2, name: 'Maria Silva', birthDate: '1830-07-22', relationship: 'Esposa' },
          { id: 3, name: 'Pedro Silva', birthDate: '1855-12-10', relationship: 'Filho' },
          { id: 4, name: 'Ana Silva', birthDate: '1858-05-03', relationship: 'Filha' }
        ]
      },
      souza: {
        name: 'Souza',
        origin: 'Exu, PE',
        founder: 'Antonio Souza',
        arrivalYear: '1870',
        image: '/placeholder-family.jpg',
        history: 'Os Souza migraram de Exu para Araripina em busca de melhores oportunidades na agricultura e pecuária. Antonio Souza trouxe consigo conhecimentos sobre cultivo no sertão e técnicas de criação adaptadas ao clima da região.',
        members: [
          { id: 5, name: 'Antonio Souza', birthDate: '1835-08-20', relationship: 'Fundador' },
          { id: 6, name: 'Rosa Souza', birthDate: '1840-11-12', relationship: 'Esposa' },
          { id: 7, name: 'José Souza', birthDate: '1865-02-28', relationship: 'Filho' }
        ]
      },
      lima: {
        name: 'Lima',
        origin: 'Juazeiro, CE',
        founder: 'Manoel Lima',
        arrivalYear: '1880',
        image: '/placeholder-family.jpg',
        history: 'A família Lima chegou a Araripina vinda do Ceará, especificamente da região de Juazeiro. Manoel Lima era artesão especializado em trabalhos com couro e estabeleceu uma das primeiras oficinas de selaria da cidade.',
        members: [
          { id: 8, name: 'Manoel Lima', birthDate: '1845-06-10', relationship: 'Fundador' },
          { id: 9, name: 'Isabel Lima', birthDate: '1850-09-15', relationship: 'Esposa' }
        ]
      }
    }
    
    familyData.value = dadosMock[familyId] ?? dadosMock.silva
    
    // Atualizar breadcrumb com nome da família
    if (breadcrumbItems.value[2]) {
      breadcrumbItems.value[2].title = `Família ${familyData.value.name}`
    }
    
  } catch (error) {
    console.error('Erro ao carregar dados da família:', error)
  } finally {
    loading.value = false
  }
}

// Métodos de navegação e ações
const verMembro = (membroId: number): void => {
  navigateTo(`/pessoas/${membroId}`)
}

const adicionarMembro = (): void => {
  // TODO: Implementar modal de adicionar membro
  console.log('Adicionar membro à família', familyData.value.name)
}

const editarFamilia = (): void => {
  navigateTo(`/familias/${familyId}/editar`)
}

const verArvore = (): void => {
  // TODO: Implementar visualização da árvore genealógica
  console.log('Ver árvore genealógica da família', familyData.value.name)
}

// Funções utilitárias
const formatarData = (data: string): string => {
  const dataObj = new Date(data)
  return dataObj.toLocaleDateString('pt-BR', {
    year: 'numeric',
    month: 'short',
    day: 'numeric'
  })
}

const calcularGeracoes = (): number => {
  // Lógica simples para calcular gerações baseada nos membros
  const anoAtual = new Date().getFullYear()
  const anoFundacao = parseInt(familyData.value.arrivalYear)
  return Math.ceil((anoAtual - anoFundacao) / 25) // Aproximadamente 25 anos por geração
}

// Lifecycle
onMounted(() => {
  buscarDadosFamilia()
})
</script>