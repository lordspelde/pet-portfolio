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

const gridVisible = ref(false)
const uploadVisible = ref(false)

const images = ref([])
const features = ref('N/A')
const descriptors = ref('N/A')
const loading = ref(true)

function toggleGrid() {
  gridVisible.value = !gridVisible.value
}

function toggleUpload() {
  uploadVisible.value = !uploadVisible.value
}

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
  <!-- Main card -->
  <v-container>
    <v-card
      elevation="2"
      class="rounded-xl overflow-hidden mx-auto"
      max-width="40%"
    >
      <v-card-title class="text-h3 ml-4 mb-4">{{ name }}</v-card-title>
      <v-carousel crossfade=true cycle=true hide-delimiters=true :key="images.length">
        <v-carousel-item v-for="img in images" :src="img"/>
      </v-carousel>

      <v-card-text>
        <h4>About me</h4>
        <p class="text-body-1">{{ descriptors }}</p>
        <h4>Features</h4>
        <p class="text-body-1">{{ features }}</p>
      </v-card-text>

      <v-card-actions>
        <v-spacer/>
        <v-btn color="primary" variant="tonal" size="large" @click="toggleGrid">
          See more of {{ name }}
        </v-btn>
        <v-btn color="secondary" variant="tonal" size="large" @click="toggleUpload">
          Upload additional photos
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-container>

  <!-- Photo grid -->
  <v-dialog
    v-model="gridVisible"
    width="85%"
    height="85%"
    @click:outside="toggleGrid"
    transition="dialog-bottom-transition"
    :scrim="false"
  >
    <v-card>
      <v-toolbar color="transparent" class="text-white px-4">
        <v-toolbar-title>Photo Gallery</v-toolbar-title>
        <v-spacer/>
        <v-btn icon="mdi-close" variant="text" @click="gridVisible = false"/>
      </v-toolbar>

      <v-card-text>
        <v-container fluid class="masonry-grid">
          <v-hover
            v-slot="{ isHovering, props }"
            v-for="(img, i) in images"
            :key="i"
          >
            <v-card
              v-bind="props"
              :elevation="isHovering ? 12 : 2"
              class="rounded-lg overflow-hidden masonry-item"
            >
              <v-img :src="img"/>
            </v-card>
          </v-hover>
        </v-container>
      </v-card-text>
    </v-card>
  </v-dialog>

  <!-- Upload additional photos -->
  <v-dialog v-model="uploadVisible">
    <v-card>
      <v-card-title>Upload Photo</v-card-title>
      <v-card-text>
        <p>Upload additional photos of {{ name }}</p>
      </v-card-text>
    </v-card>
  </v-dialog>

  <!-- Loading indicator -->
  <v-container v-if="loading" class="text-center">
    <v-progress-circular indeterminate color="primary"></v-progress-circular>
    <p>Loading pet details...</p>
  </v-container>
</template>

<style scoped>
.masonry-grid {
  column-count: 6;
  gap: 16px;
  width: 100%;
}

.masonry-item {
  display: inline-block;
  width: 100%;
}

@media (max-width: 1200px) {
  .masonry-grid {
    column-count: 4;
  }
}

@media (max-width: 600px) {
  .masonry-grid {
    column-count: 3;
  }
}
</style>