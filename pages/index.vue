<template>
  <div class="home-page">
    <!-- Hero Section -->
    <HeroSection />
    
    <!-- Approach Section -->
    <ApproachSection />
    <ProjectsSection />
    <ClientsSection />
    <StyleSection />
  </div>
</template>

<script setup>
import HeroSection from '~/components/HeroSection.vue'
import ApproachSection from '~/components/ApproachSection.vue'
import ProjectsSection from '~/components/ExpertiseSection.vue'
import ClientsSection from '~/components/ClientsSection.vue'
import StyleSection from '~/components/StyleSection.vue'
</script>

<style scoped>
.home-page {
  background-color: black;
}
</style>