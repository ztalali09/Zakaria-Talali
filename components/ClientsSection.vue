<template>
  <section class="clients-section">
    <div class="content-container">
      <!-- Section Header -->
      <div class="section-header">
        <div class="border-section">
          <h3 class="section-title">
            <span class="section-number">04/</span>
            <span class="section-text">Nos clients</span>
          </h3>
          <div class="border-bottom"></div>
        </div>
      </div>

      <!-- Logo Carousels -->
      <div class="carousels-wrapper">
        <!-- First Carousel -->
        <div class="carousel-container">
          <div class="carousel-track" :style="carouselStyle">
            <div v-for="(logo, index) in [...logos, ...logos, ...logos, ...logos, ...logos]" :key="index" class="logo-item">
              <img :src="logo.src" :alt="logo.alt" class="logo-image" />
            </div>
          </div>
        </div>

        <!-- Second Carousel (reverse direction) -->
        <div class="carousel-container second-carousel">
          <div class="carousel-track" :style="secondCarouselStyle">
            <div v-for="(logo, index) in [...logos, ...logos, ...logos, ...logos, ...logos]" :key="index" class="logo-item">
              <img :src="logo.src" :alt="logo.alt" class="logo-image" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const logos = [
  { src: '/images/clients/logo1.png', alt: 'Client 1' },
  { src: '/images/clients/logo2.png', alt: 'Client 2' },
  { src: '/images/clients/logo3.png', alt: 'Client 3' },
  { src: '/images/clients/logo4.png', alt: 'Client 4' },
  { src: '/images/clients/logo5.png', alt: 'Client 5' },
  { src: '/images/clients/logo6.png', alt: 'Client 6' },
  { src: '/images/clients/logo7.png', alt: 'Client 7' },
  { src: '/images/clients/logo8.png', alt: 'Client 8' },
  { src: '/images/clients/logo9.png', alt: 'Client 9' },
  { src: '/images/clients/logo10.png', alt: 'Client 10' },
  { src: '/images/clients/logo11.png', alt: 'Client 11' },
  { src: '/images/clients/logo12.png', alt: 'Client 12' },
  { src: '/images/clients/logo13.png', alt: 'Client 13' },
  { src: '/images/clients/logo14.png', alt: 'Client 14' },
  { src: '/images/clients/logo15.png', alt: 'Client 15' }
]

const scrollPosition = ref(0)
const secondScrollPosition = ref(-3000)
const carouselStyle = ref({ transform: 'translateX(0px)' })
const secondCarouselStyle = ref({ transform: 'translateX(-3000px)' })

const scrollCarousel = () => {
  // First carousel moves left
  scrollPosition.value -= 1
  carouselStyle.value = {
    transform: `translateX(${scrollPosition.value}px)`
  }
  
  // Second carousel moves right
  secondScrollPosition.value += 1
  secondCarouselStyle.value = {
    transform: `translateX(${secondScrollPosition.value}px)`
  }
  
  // Reset positions when we've scrolled through one set of logos
  const logoWidth = 200
  const gapWidth = 2 * 16 // 2rem gap
  const totalWidth = (logoWidth + gapWidth) * logos.length
  
  if (Math.abs(scrollPosition.value) >= totalWidth) {
    scrollPosition.value = 0
  }
  
  if (secondScrollPosition.value >= 0) {
    secondScrollPosition.value = -totalWidth
  }
}

let animationFrame

onMounted(() => {
  const animate = () => {
    scrollCarousel()
    animationFrame = requestAnimationFrame(animate)
  }
  animationFrame = requestAnimationFrame(animate)
})

onUnmounted(() => {
  if (animationFrame) {
    cancelAnimationFrame(animationFrame)
  }
})
</script>

<style scoped>
.clients-section {
  position: relative;
  width: 100%;
  padding: 0 0 12rem 0;
  background: linear-gradient(to bottom, #DCFCE7, #F0FDF4);
  overflow: hidden;
  margin-top: -2rem;
}

.content-container {
  width: 100%;
  max-width: none;
  margin: 0;
  padding: 0;
}

.section-header {
  padding-top: 2rem;
  max-width: 80rem;
  margin: 0 auto;
  padding: 0 2rem;
}

.section-title {
  font-size: 1rem;
  font-weight: 300;
  color: #166534;
  letter-spacing: 0.1em;
}

.section-number {
  color: #16A34A;
}

.section-text {
  margin-left: 0.5rem;
}

.border-bottom {
  width: 100%;
  height: 1px;
  background-color: rgba(10, 10, 10, 0.1);
  margin-top: 1rem;
}

.carousels-wrapper {
  display: flex;
  flex-direction: column;
  gap: 0;
  margin-top: 4rem;
  width: 100%;
}

.carousel-container {
  width: 100%;
  overflow: hidden;
  position: relative;
}

.second-carousel {
  margin-top: 0;
}

.carousel-track {
  display: flex;
  gap: 2rem;
  padding: 2rem 0;
  width: max-content;
  will-change: transform;
}

.logo-item {
  flex: 0 0 200px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.logo-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  filter: grayscale(100%);
  transition: filter 0.3s ease;
}

.logo-item:hover .logo-image {
  filter: grayscale(0%);
}

@media (max-width: 640px) {
  .logo-item {
    flex: 0 0 150px;
    height: 75px;
  }
}
</style> 