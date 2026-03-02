<script setup>

</script>

<template>
    <div class="d-flex align-center justify-center mt-8" style="gap: 2rem;">
        <div class="text-block" style="max-width: 320px;">
            <h2 class="mb-2">Makerspace Inventory Manager</h2>
            <p class="mb-4">
                There's not much here yet, but in the future there will be more content and features
                for tracking the Makerspace's inventory of various components.
            </p>
            <p>
                Until then, I've filled the space with this AI-generated cat image carousel to meet the requirements :)
            </p>
        </div>
        
        <div class="carousel-wrapper">
            <v-carousel crossfade="true" cycle="true" hide-delimiters="true" height="500">
                <v-carousel-item src="ai-cat-1.png" cover></v-carousel-item>
                <v-carousel-item src="ai-cat-2.png" cover></v-carousel-item>
                <v-carousel-item src="ai-cat-3.png" cover></v-carousel-item>
            </v-carousel>
            <div class="carousel-fade"></div>
        </div>
    </div>
</template>

<style scoped>
.carousel-wrapper {
    position: relative;
    width: 600px;
}

.v-carousel {
    border-radius: 18px;
    overflow: hidden;
    box-shadow: 0 4px 24px rgba(0,0,0,0.12);
}

.carousel-fade {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    height: 80px;
    pointer-events: none;
    background: linear-gradient(to bottom, rgba(255,255,255,0) 0%, var(--fade-bg, #fff) 100%);
    border-bottom-left-radius: 18px;
    border-bottom-right-radius: 18px;
}
</style>