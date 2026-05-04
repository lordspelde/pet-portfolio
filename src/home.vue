<script setup>
import { onMounted, ref } from 'vue';

const URL = '/api'

const loading = ref(true)
const pets = ref({})

onMounted(async () => {
  try {
    // grab all pets
    const response = await fetch(`${URL}/pets`)

    if (!response.ok) {
      console.error(`Failed to fetch pets`)
    }

    const petData = await response.json()
    
    for (const pet of petData) {
      pets.value[pet.id] = pet
    }
  } catch (error) {
    console.error(`Error fetching pet data`, error)
  } finally {
    loading.value = false
  }
})
</script>

<template>
    <div class="d-flex align-center justify-center mt-8">
        <div class="text-block" style="max-width: 320px;">
            <h2 class="mb-2">Pet Portfolio</h2>
            <p class="mb-4">
                Welcome to the Pet Portfolio. Here you can find some information about my pets as well as plenty of pictures.
            </p>
        </div>
    </div>

    <v-container v-if="!loading" class="mt-8">
        <v-row>
            <v-col v-for="pet in pets" :key="pet.id" cols="12" sm="6" md="4" lg="3">
                <v-card :href="'#/' + pet.name.toLowerCase()" class="rounded-xl overflow-hidden">
                    <v-card-title class="text-h5">{{ pet.name }}</v-card-title>
                    <v-img :src="pet.photos[0]" height="400px"></v-img>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<style scoped>

</style>