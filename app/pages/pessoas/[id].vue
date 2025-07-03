<template>
    <div>
        <v-container>
            <header>
                <v-row>
                    <v-col cols="12">
                        <v-btn variant="text" prepend-icon="mdi-arrow-left" @click="$router.back()">
                            Voltar
                        </v-btn>
                    </v-col>
                </v-row>
            </header>
 
            <main>
                <v-row v-if="loading" justify="center">
                    <v-col cols="12" class="text-center">
                        <v-progress-circular indeterminate size="64" />
                    </v-col>
                </v-row>

                <v-row v-else-if="person">
                    <v-col cols="12" md="4">
                        <aside>
                            <v-card>
                                <v-card-text class="text-center">
                                    <v-avatar size="120" class="mb-4">
                                        <v-img v-if="person.photo" :src="person.photo"
                                            :alt="`Foto de ${person.name}`" />
                                        <v-icon v-else size="80">mdi-account</v-icon>
                                    </v-avatar>
                                    <h1 class="text-h4 mb-2">{{ person.name }}</h1>
                                    <p class="text-subtitle1 text-medium-emphasis">
                                        {{ person.birthDate }} - {{ person.deathDate || 'Presente' }}
                                    </p>
                                </v-card-text>
                            </v-card>
                        </aside>
                    </v-col>

                    <v-col cols="12" md="8">
                        <section>
                            <v-card>
                                <v-card-title>
                                    <h2>Informações Pessoais</h2>
                                </v-card-title>
                                <v-card-text>
                                    <v-row>
                                        <v-col cols="12" sm="6">
                                            <v-text-field label="Nome Completo" :model-value="person.fullName" readonly
                                                variant="outlined" />
                                        </v-col>
                                        <v-col cols="12" sm="6">
                                            <v-text-field label="Data de Nascimento" :model-value="person.birthDate"
                                                readonly variant="outlined" />
                                        </v-col>
                                        <v-col cols="12" sm="6">
                                            <v-text-field label="Local de Nascimento" :model-value="person.birthPlace"
                                                readonly variant="outlined" />
                                        </v-col>
                                        <v-col cols="12" sm="6">
                                            <v-text-field label="Profissão" :model-value="person.profession" readonly
                                                variant="outlined" />
                                        </v-col>
                                    </v-row>
                                </v-card-text>
                            </v-card>
                        </section>

                        <section class="mt-4">
                            <v-card>
                                <v-card-title>
                                    <h2>Família</h2>
                                </v-card-title>
                                <v-card-text>
                                    <v-row>
                                        <v-col cols="12" md="6">
                                            <h3 class="text-h6 mb-3">Pais</h3>
                                            <v-list>
                                                <v-list-item v-for="parent in person.parents" :key="parent.id"
                                                    :to="`/pessoas/${parent.id}`">
                                                    <template #prepend>
                                                        <v-icon>mdi-account-supervisor</v-icon>
                                                    </template>
                                                    <v-list-item-title>{{ parent.name }}</v-list-item-title>
                                                </v-list-item>
                                            </v-list>
                                        </v-col>
                                        <v-col cols="12" md="6">
                                            <h3 class="text-h6 mb-3">Filhos</h3>
                                            <v-list>
                                                <v-list-item v-for="child in person.children" :key="child.id"
                                                    :to="`/pessoas/${child.id}`">
                                                    <template #prepend>
                                                        <v-icon>mdi-account-child</v-icon>
                                                    </template>
                                                    <v-list-item-title>{{ child.name }}</v-list-item-title>
                                                </v-list-item>
                                            </v-list>
                                        </v-col>
                                    </v-row>
                                </v-card-text>
                            </v-card>
                        </section>
                    </v-col>
                </v-row>

                <v-row v-else>
                    <v-col cols="12" class="text-center">
                        <v-alert type="error" variant="outlined">
                            Pessoa não encontrada
                        </v-alert>
                    </v-col>
                </v-row>
            </main>
        </v-container>
    </div>
</template>

<script setup>
const route = useRoute()
const loading = ref(true)
const person = ref(null)

const fetchPerson = async () => {
    try {
        loading.value = true
        // Simulated API call
        await new Promise(resolve => setTimeout(resolve, 1000))

        // Mock data
        person.value = {
            id: route.params.id,
            name: 'João Silva',
            fullName: 'João da Silva Santos',
            birthDate: '15/03/1950',
            deathDate: null,
            birthPlace: 'Araripina, PE',
            profession: 'Agricultor',
            photo: null,
            parents: [
                { id: '1', name: 'Maria Silva' },
                { id: '2', name: 'José Santos' }
            ],
            children: [
                { id: '3', name: 'Ana Silva' },
                { id: '4', name: 'Pedro Silva' }
            ]
        }
    } catch (error) {
        person.value = null
    } finally {
        loading.value = false
    }
}

onMounted(fetchPerson)
</script>