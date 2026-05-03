<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  name: { type: String, required: true }, // displayed on the page
  id: { type: String, required: true }, // WIP / TODO used for querying the rest of the details from a TBD server

  // stuff to be replaced by a database
//   images: { type: Array, default: () => [] },
//   features: { type: Array, default: () => ['N/A'] },
//   descriptors: { type: Array, default: () => ['N/A'] },
})

const images = ref([])
const features = ref('N/A')
const descriptors = ref('N/A')
const loading = ref(true)

onMounted(async () => {
  try {
    // grab specific pet
    const response = await fetch(`http://localhost:3000/pets/${props.id}`)

    if (!response.ok) {
      console.error(`Failed to fetch pet: ${props.name}`)
    }

    const petData = await response.json()

    images.value = petData.photos || []
    features.value = petData.features || 'N/A'
    descriptors.value = petData.descriptors || 'N/A'
  } catch (error) {
    console.error(`Error fetching pet data for ${props.name}`, error)
  } finally {
    loading.value = false
  }
})
</script>

<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" md="8">
        <v-card elevation="2" class="rounded-xl overflow-hidden">
          <v-card-title class="text-h3 ml-4 mb-4">{{ name }}</v-card-title>
          <v-carousel crossfade=true cycle=true hide-delimiters=true>
            <v-carousel-item v-for="img in images" :src="img"/>
          </v-carousel>

          <v-card-text>
            <h4>About me</h4>
            <p class="text-body-1">{{ descriptors }}</p>
            <h4>Features</h4>
            <p class="text-body-1">{{ features }}</p>
          </v-card-text>

          <v-card-actions>
            <v-btn color="primary" variant="tonal" block size="large">
              See more of {{ name }}
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<style scoped></style>