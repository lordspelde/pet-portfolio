<script setup>
import { ref, computed } from 'vue'
import home from './home.vue'
import about from './about.vue'

import luna from './pets/luna.vue'
import layla from './pets/layla.vue'
import harry from './pets/harry.vue'
import jovi from './pets/jovi.vue'

const routes = {
    '/': home,
    '/about': about,
    '/luna': luna,
    '/layla': layla,
    '/harry': harry,
    '/jovi': jovi,
}

const currentPath = ref(window.location.hash);
const drawer = ref(false);

window.addEventListener('hashchange', () => {
    currentPath.value = window.location.hash;
});

const currentView = computed(() => {
    return routes[currentPath.value.slice(1) || '/'] || NotFound;
})
</script>

<template>
    <v-app>
        <v-navigation-drawer v-model="drawer" color="blue-lighten-5">
            <v-list-item prepend-icon="mdi-home" href="/#" title="Home" @click="drawer = !drawer"/>
            <v-list-item prepend-icon="mdi-information-variant-circle-outline" href="#/about" title="About" @click="drawer = !drawer"/>
            <v-list-item prepend-icon="mdi-paw" href="/#/luna" title="Luna" @click="drawer = ~drawer"/>
            <v-list-item prepend-icon="mdi-paw" href="/#/layla" title="Layla" @click="drawer = ~drawer"/>
            <v-list-item prepend-icon="mdi-paw" href="/#/harry" title="Harry" @click="drawer = ~drawer"/>
            <v-list-item prepend-icon="mdi-paw" href="/#/jovi" title="Jovi" @click="drawer = ~drawer"/>
        </v-navigation-drawer>
        <v-app-bar class="text-orange-4 mainHeader" color="blue-lighten-4">
            <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
            <v-app-bar-title>Pet Portfolio</v-app-bar-title>
        </v-app-bar>
        <v-main>
            <component :is="currentView"></component>
        </v-main>
        <v-footer app=true>Copyright 2026</v-footer>
    </v-app>
</template>