'use client'

<template>
  <section class="expertise-section">
    <!-- Content Container -->
    <div class="content-container">
      <!-- Section Header -->
      <div class="section-header">
        <div class="border-section">
          <h3 class="section-title">
            <span class="section-number">03/</span>
            <span class="section-text">Our expertise</span>
          </h3>
          <div class="border-bottom"></div>
        </div>
      </div>

      <!-- Main Content -->
      <div class="main-content">
        <div class="heading-container">
          <AnimatedText 
            text="Discover our comprehensive range of digital expertise"
            class="main-heading"
          />
        </div>

        <!-- Expertise Grid -->
        <div class="expertise-grid">
          <div
            v-for="(expertise, index) in expertiseList" 
            :key="index"
            class="expertise-item"
            @mouseenter="handleMouseEnter(index)"
            @mouseleave="handleMouseLeave"
          >
            <h2 class="expertise-title">{{ expertise.title }}</h2>
            <p class="expertise-description">{{ expertise.description }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Image Preview -->
    <div 
      v-for="(expertise, index) in expertiseList" 
      :key="index"
      ref="previewContainer"
      class="preview-container"
      :class="{ 'preview-active': isPreviewActive && currentIndex === index }"
      :style="previewStyle"
    >
      <div class="preview-item">
        <img 
          :src="expertise.image" 
          :alt="expertise.title"
          class="preview-image"
          @load="handleImageLoad"
        />
      </div>
    </div>

    <!-- Custom Cursor -->
    <div 
      class="custom-cursor"
      :class="{ 'cursor-active': isPreviewActive }"
      :style="cursorStyle"
    ></div>
    <div 
      class="cursor-trail"
      :style="cursorStyle"
    ></div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import AnimatedText from './AnimatedText.vue'

const expertiseList = [
  {
    title: "Frontend Development",
    description: "Modern web interfaces with Vue.js & React",
    image: "/images/bbbb.png",
    color: "#86EFAC",
    route: "/expertise/frontend"
  },
  {
    title: "E-commerce Solutions",
    description: "Custom Shopify development & optimization",
    image: "/images/cccc.png",
    color: "#4ADE80",
    route: "/expertise/ecommerce"
  },
  {
    title: "UI/UX Design",
    description: "User-centered design & interactive experiences",
    image: "/images/nnnn.png",
    color: "#22C55E",
    route: "/expertise/design"
  },
  {
    title: "Performance Optimization",
    description: "Speed optimization & technical SEO",
    image: "/images/ecommercesolutions.png",
    color: "#16A34A",
    route: "/expertise/performance"
  }
]

// State
const isPreviewActive = ref(false)
const currentIndex = ref(0)
const mousePosition = ref({ x: 0, y: 0 })
const mouseVelocity = ref({ x: 0, y: 0 })
const lastMousePosition = ref({ x: 0, y: 0 })
const previewContainer = ref(null)

// Computed
const previewStyle = computed(() => {
  // Calculer la distance de décalage basée sur la vitesse
  const speed = Math.sqrt(mouseVelocity.value.x * mouseVelocity.value.x + mouseVelocity.value.y * mouseVelocity.value.y)
  const maxOffset = 20
  const offset = Math.min(speed * 0.5, maxOffset)
  
  // Calculer le décalage en X et Y basé sur la direction
  const translateX = mouseVelocity.value.x * 0.1
  const translateY = mouseVelocity.value.y * 0.1
  
  return {
    left: `${mousePosition.value.x}px`,
    top: `${mousePosition.value.y}px`,
    transform: `translate(calc(-50% + ${translateX}px), calc(-50% + ${translateY}px))`
  }
})

const cursorStyle = computed(() => ({
  left: `${mousePosition.value.x}px`,
  top: `${mousePosition.value.y}px`
}))

// Methods
const handleMouseEnter = (index) => {
  currentIndex.value = index
  isPreviewActive.value = true
}

const handleMouseLeave = () => {
  isPreviewActive.value = false
}

const handleImageLoad = (event) => {
  console.log('Image loaded:', event.target.src)
}

// Mouse tracking
const handleMouseMove = (e) => {
  const now = Date.now()
  const deltaX = e.clientX - lastMousePosition.value.x
  const deltaY = e.clientY - lastMousePosition.value.y
  
  mouseVelocity.value = {
    x: deltaX,
    y: deltaY
  }
  
  mousePosition.value = {
    x: e.clientX,
    y: e.clientY
  }
  
  lastMousePosition.value = {
    x: e.clientX,
    y: e.clientY
  }
}

// Lifecycle
onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove)
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
})
</script>

<style scoped>
.expertise-section {
  position: relative;
  width: 100%;
  min-height: 100vh;
  background: linear-gradient(to bottom, #F0FDF4, #DCFCE7);
  border-radius: 2.5rem 2.5rem 0 0;
  margin-top: -2rem;
  overflow: hidden;
  padding: 2rem 1rem;
}

@media (min-width: 640px) {
  .expertise-section {
    padding: 3rem 2rem;
  }
}

@media (min-width: 1024px) {
  .expertise-section {
    padding: 4rem 3rem;
  }
}

.content-container {
  max-width: 1280px;
  margin: 0 auto;
  position: relative;
  z-index: 1;
}

.section-header {
  margin-bottom: 2rem;
}

@media (min-width: 640px) {
  .section-header {
    margin-bottom: 3rem;
  }
}

.border-section {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.section-title {
  font-size: 1.5rem;
  font-weight: 500;
  color: #333;
}

@media (min-width: 640px) {
  .section-title {
    font-size: 2rem;
  }
}

.section-number {
  color: #00FFB2;
  margin-right: 0.5rem;
}

.border-bottom {
  flex-grow: 1;
  height: 1px;
  background: #00FFB2;
  opacity: 0.3;
}

.main-content {
  margin-top: 2rem;
}

.heading-container {
  margin-bottom: 3rem;
  max-width: 800px;
}

.main-heading {
  font-size: 2rem;
  line-height: 1.2;
  font-weight: 700;
  color: #333;
}

@media (min-width: 640px) {
  .main-heading {
    font-size: 2.5rem;
  }
}

@media (min-width: 1024px) {
  .main-heading {
    font-size: 3rem;
  }
}

.expertise-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
  margin-top: 2rem;
}

@media (min-width: 640px) {
  .expertise-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 2.5rem;
  }
}

@media (min-width: 1024px) {
  .expertise-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 3rem;
  }
}

.expertise-item {
  background: rgba(255, 255, 255, 0.8);
  padding: 2rem;
  border-radius: 1rem;
  transition: all 0.3s ease;
  cursor: pointer;
  backdrop-filter: blur(10px);
}

@media (min-width: 640px) {
  .expertise-item {
    padding: 2.5rem;
  }
}

.expertise-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.expertise-title {
  font-size: 1.5rem;
  font-weight: 600;
  color: #333;
  margin-bottom: 1rem;
}

@media (min-width: 640px) {
  .expertise-title {
    font-size: 1.75rem;
  }
}

.expertise-description {
  font-size: 1rem;
  color: #666;
  line-height: 1.6;
}

@media (min-width: 640px) {
  .expertise-description {
    font-size: 1.125rem;
  }
}

.preview-container {
  position: fixed;
  pointer-events: none;
  z-index: 10;
  width: 200px;
  height: 100px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

@media (min-width: 640px) {
  .preview-container {
    width: 250px;
    height: 125px;
  }
}

@media (min-width: 1024px) {
  .preview-container {
    width: 300px;
    height: 150px;
  }
}

.preview-active {
  opacity: 1;
}

.preview-item {
  width: 100%;
  height: 100%;
  border-radius: 1rem;
  overflow: hidden;
}

.preview-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.custom-cursor {
  position: fixed;
  width: 20px;
  height: 20px;
  border: 2px solid #00FFB2;
  border-radius: 50%;
  pointer-events: none;
  transform: translate(-50%, -50%);
  transition: none;
  z-index: 100;
}

.cursor-active {
  width: 30px;
  height: 30px;
  transition: none;
}

.cursor-trail {
  position: fixed;
  width: 8px;
  height: 8px;
  background: rgba(0, 255, 178, 0.3);
  border-radius: 50%;
  pointer-events: none;
  transform: translate(-50%, -50%);
  z-index: 99;
  filter: blur(2px);
  animation: fadeOut 0.5s ease-out forwards;
}

@keyframes fadeOut {
  from {
    opacity: 0.5;
    transform: translate(-50%, -50%) scale(1);
  }
  to {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.5);
  }
}

.cursor-label {
  display: none;
}

.label-active {
  display: none;
}
</style> 