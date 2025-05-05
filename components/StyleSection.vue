<template>
  <section class="style-section relative w-full min-h-screen overflow-hidden bg-[#0A0A0A] rounded-t-[2.5rem] -mt-8">
    <!-- Content Container -->
    <div class="relative z-20 container mx-auto px-4 py-12">
      <!-- Section Number and Title -->
      <div class="pt-4 md:pt-1 max-w-7xl mx-auto">
        <div class="border-section">
          <h3 class="text-base md:text-lg font-light text-white tracking-wider">
            05/ <span class="ml-2">Nos Services</span>
          </h3>
          <div class="border-bottom mt-4"></div>
        </div>
      </div>
      
      <!-- Main Content -->
      <div class="mt-14">
        <div class="w-full max-w-7xl mx-auto">
          <!-- Main Heading -->
          <div class="mb-20">
            <AnimatedText 
              text="Vectors : des talents à la polyvalence unique au service de votre communication."
              class="text-4xl sm:text-5xl md:text-6xl lg:text-[72px] font-normal text-white leading-[1.15] tracking-normal max-w-[800px]"
            />
          </div>

          <!-- Description -->
          <div class="flex flex-col lg:flex-row justify-between items-start gap-20">
            <div class="lg:w-1/2">
              <a href="#" class="inline-flex items-center px-8 py-4 border border-white/20 rounded-full text-white hover:bg-white/10 transition-colors duration-300">
                En savoir plus sur l'agence
                <span class="ml-2 w-2 h-2 rounded-full bg-[#FF4D00]"></span>
              </a>
            </div>
            <div class="lg:w-1/2">
              <p class="text-white/70 text-xl leading-relaxed">
                Consultant(e)s en communication, consultant(e)s veille, directeurs(rices) artistiques, planneurs(euses) stratégiques, vidéastes, data scientists, développeurs(euses), chef(fe)s de projet... toutes les expertises requises dans le monde de l'instantanéité.
              </p>
            </div>
          </div>

          <!-- Image Slider -->
          <div 
            class="c-team-slider is-inview" 
            data-scroll="" 
            data-scroll-css-progress="" 
            data-scroll-event-progress="team-slider:progress" 
            data-scroll-offset="0%, 100%"
            :style="{ '--progress': scrollProgress }"
          >
            <div class="c-team-slider__track">
                <div 
                  v-for="(image, index) in images" 
                  :key="index"
                class="c-team-slider__item"
              >
                <img 
                  :src="image.src" 
                  :alt="image.alt"
                  :srcset="`${image.src} 1x, ${image.src} 2x`"
                  class="c-team-slider__image"
                  onerror="this.setAttribute('data-error', 1)"
                />
              </div>
              <!-- Action Card -->
              <div class="c-team-slider__item">
                <article class="c-action-card">
                  <a href="#" class="c-action-card__link"></a>
                  <div class="c-action-card__background">
                    <div class="c-grid-background">
                      <div class="c-grid-background__grid"></div>
                    </div>
                  </div>
                  <div class="c-action-card__content">
                    <p class="c-action-card__title">Envie de rejoindre l'aventure ?</p>
                    <div class="c-action-card__button">
                      <button class="c-button -background-orange -color-black -has-arrow">
                        <span class="c-button__label">Nos offres d'emploi</span>
                        <div class="c-button__dot-arrow">
                          <div class="c-dot-arrow -color-orange -arrow-default -type-default">
                            <span class="c-dot-arrow__background"></span>
                            <span class="c-dot-arrow__arrow">
                              <svg width="14" height="18" viewBox="0 0 14 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <g>
                                  <g>
                                    <line x1="3.59599" y1="16.4952" x2="12.0812" y2="8.00999" stroke="#ffffff" stroke-width="2.79999"/>
                                    <line x1="3.87874" y1="1.22175" x2="12.364" y2="9.70697" stroke="#ffffff" stroke-width="2.79999"/>
                                  </g>
                                </g>
                              </svg>
                            </span>
                          </div>
                        </div>
                      </button>
                    </div>
                </div>
                </article>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import AnimatedText from './AnimatedText.vue'
import { ref, onMounted, onUnmounted } from 'vue'
import { useScroll } from '@vueuse/core'

const images = [
  { 
    src: '/images/1.png', 
    alt: 'Team meeting in office'
  },
  { 
    src: '/images/3.png', 
    alt: 'Team collaboration'
  },
  { 
    src: '/images/4.png', 
    alt: 'Team working on computers'
  },
  { 
    src: '/images/5.png', 
    alt: 'Team discussion'
  }
]

const scrollProgress = ref(0)
const isCarouselActive = ref(false)
const hasReachedEnd = ref(false)
const lastScrollPosition = ref(0)
const hasCompletedScroll = ref(false)
let isScrollLocked = false
let snapTimeout = null

// Fonction pour vérifier si le carrousel est centré dans le viewport
const isCarouselCentered = (rect) => {
  const viewportHeight = window.innerHeight
  const elementCenter = rect.top + rect.height / 2
  const viewportThreshold = viewportHeight * 0.4
  
  return elementCenter <= viewportThreshold
}

// Fonction pour vérifier si la dernière image est visible
const isLastImageVisible = () => {
  const track = document.querySelector('.c-team-slider__track')
  if (!track) return false
  
  const lastImage = track.lastElementChild
  if (!lastImage) return false
  
  const lastImageRect = lastImage.getBoundingClientRect()
  const viewportWidth = window.innerWidth
  
  // Vérifie si la dernière image est complètement visible dans le viewport
  // avec une petite marge pour éviter les problèmes de précision
  const margin = 20 // pixels de marge
  return lastImageRect.right <= (viewportWidth - margin) && lastImageRect.left >= margin
}

// Fonction pour calculer la position maximale du scroll
const calculateMaxScroll = () => {
  const track = document.querySelector('.c-team-slider__track')
  if (!track) return 1

  const lastImage = track.lastElementChild
  if (!lastImage) return 1

  const trackWidth = track.scrollWidth
  const viewportWidth = window.innerWidth
  const lastImageWidth = lastImage.offsetWidth

  // Calcule la position maximale pour que la dernière image soit complètement visible
  return 1 - (lastImageWidth / trackWidth)
}

// Fonction pour bloquer/débloquer le scroll
const toggleScrollLock = (lock) => {
  if (lock && !isScrollLocked) {
    lastScrollPosition.value = window.scrollY
    document.body.style.overflow = 'hidden'
    document.body.style.position = 'fixed'
    document.body.style.width = '100%'
    document.body.style.top = `-${lastScrollPosition.value}px`
    isScrollLocked = true
  } else if (!lock && isScrollLocked) {
    document.body.style.overflow = ''
    document.body.style.position = ''
    document.body.style.width = ''
    document.body.style.top = ''
    window.scrollTo(0, lastScrollPosition.value)
    isScrollLocked = false
  }
}

const snapToNearestImage = () => {
  const stepSize = 0.02
  const currentStep = Math.round(scrollProgress.value / stepSize) * stepSize
  scrollProgress.value = Math.min(1, Math.max(0, currentStep))
}

const debouncedSnap = () => {
  if (snapTimeout) clearTimeout(snapTimeout)
  snapTimeout = setTimeout(() => {
    snapToNearestImage()
  }, 100)
}

const updateCarousel = (event) => {
  const slider = document.querySelector('.c-team-slider')
  const track = document.querySelector('.c-team-slider__track')
  
  if (!slider || !track) return

  const sliderRect = slider.getBoundingClientRect()
  const isCentered = isCarouselCentered(sliderRect)
  const lastImageVisible = isLastImageVisible()
  const maxScroll = calculateMaxScroll()
  
  // Si le scroll a déjà été complété, on ne fait rien
  if (hasCompletedScroll.value) {
    return
  }
  
  if (isCentered && !lastImageVisible) {
    isCarouselActive.value = true
    toggleScrollLock(true)
    
    const delta = event.deltaY
    const stepSize = 0.02
    
    if (delta > 0) {
      scrollProgress.value = Math.min(maxScroll, scrollProgress.value + stepSize)
      event.preventDefault()
      
      // Si on atteint la position maximale, on marque le scroll comme complété
      if (scrollProgress.value >= maxScroll) {
        hasCompletedScroll.value = true
        isCarouselActive.value = false
        toggleScrollLock(false)
      }
    } else {
      if (scrollProgress.value > 0) {
        scrollProgress.value = Math.max(0, scrollProgress.value - stepSize)
        event.preventDefault()
      }
    }

    debouncedSnap()
  } else if (lastImageVisible) {
    // Quand la dernière image est visible, on marque le scroll comme complété
    hasCompletedScroll.value = true
    if (isCarouselActive.value) {
      isCarouselActive.value = false
      toggleScrollLock(false)
    }
  } else {
    if (isCarouselActive.value) {
      isCarouselActive.value = false
      toggleScrollLock(false)
    }
  }
}

const handleScroll = () => {
  const slider = document.querySelector('.c-team-slider')
  if (!slider) return

  const sliderRect = slider.getBoundingClientRect()
  const lastImageVisible = isLastImageVisible()
  
  if (sliderRect.bottom < 0 || sliderRect.top > window.innerHeight) {
    // On ne réinitialise pas le scroll si le carrousel a déjà été complété
    if (!hasCompletedScroll.value) {
      scrollProgress.value = 0
      isCarouselActive.value = false
      toggleScrollLock(false)
      if (snapTimeout) clearTimeout(snapTimeout)
    }
  } else if (lastImageVisible) {
    // Si la dernière image est visible, on marque le scroll comme complété
    hasCompletedScroll.value = true
    if (isCarouselActive.value) {
      isCarouselActive.value = false
      toggleScrollLock(false)
    }
  }
}

onMounted(() => {
  window.addEventListener('wheel', updateCarousel, { passive: false })
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('wheel', updateCarousel)
  window.removeEventListener('scroll', handleScroll)
  toggleScrollLock(false)
  if (snapTimeout) clearTimeout(snapTimeout)
})
</script>

<style scoped>
.style-section {
  position: relative;
  width: 100%;
  min-height: 300vh !important; /* Force more scroll space */
  color: white;
  font-family: 'HK Grotesk', sans-serif;
}

.border-section {
  position: relative;
}

.border-bottom {
  width: 100%;
  height: 1px;
  background-color: rgba(255, 255, 255, 0.2);
}

.c-team-slider {
  position: relative;
  width: 100vw;
  margin-left: 50%;
  transform: translateX(-50%);
  overflow: hidden;
  height: 100vh;
  padding: 0;
  transition: transform 0.3s ease-out;
}

.c-team-slider.is-active {
  z-index: 10;
}

.c-team-slider__track {
  display: flex;
  position: sticky;
  top: 50%;
  transform: translateY(-50%);
  padding-left: max(4rem, calc((100vw - 300px)));
  padding-right: max(4rem, calc((100vw - 300px)));
  gap: 0;
  will-change: transform;
  transform: translateX(calc(var(--progress, 0) * -100%));
  transition: transform 0.6s cubic-bezier(0.4, 0.1, 0.2, 1);
  scroll-snap-type: x mandatory;
}

.c-team-slider__item {
  flex: 0 0 auto;
  width: 350px;
  height: 525px;
  position: relative;
  z-index: 1;
  opacity: 1;
  transition: all 0.6s cubic-bezier(0.4, 0.1, 0.2, 1);
  scroll-snap-align: start;
  transform: rotate(var(--rotation, 0deg));
  margin: 0;
}

.c-team-slider__item:nth-child(1) { --rotation: -3deg; }
.c-team-slider__item:nth-child(2) { --rotation: 2deg; }
.c-team-slider__item:nth-child(3) { --rotation: -2deg; }
.c-team-slider__item:nth-child(4) { --rotation: 3deg; }
.c-team-slider__item:nth-child(5) { --rotation: -2deg; }

.c-team-slider__image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 0;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease-out;
}

.c-team-slider__item:hover {
  transform: rotate(var(--rotation, 0deg)) translateY(-10px);
  z-index: 2;
}

.c-team-slider__item:hover .c-team-slider__image {
  transform: scale(1.05);
}

.c-action-card {
  height: 100%;
  background: #FF4D00;
  border-radius: 1rem;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

.c-action-card__background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: 
    linear-gradient(rgba(255, 255, 255, 0.1) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.1) 1px, transparent 1px);
  background-size: 20px 20px;
  opacity: 0.5;
  z-index: 1;
}

.c-action-card__content {
  position: relative;
  z-index: 2;
}

.c-action-card__title {
  color: white;
  font-size: clamp(1.5rem, 3vw, 2.5rem);
  font-weight: 600;
  margin-bottom: 1.5rem;
}

.c-button {
  display: inline-flex;
  align-items: center;
  padding: 1rem 2rem;
  background: white;
  border-radius: 3rem;
  border: none;
  cursor: pointer;
  transition: transform 0.3s ease;
  font-size: clamp(1rem, 1.5vw, 1.25rem);
}

.c-button:hover {
  transform: translateY(-2px);
}

.c-button__label {
  margin-right: 0.5rem;
  color: black;
}

.c-grid-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0.1;
  pointer-events: none;
}

.c-grid-background__grid {
  width: 100%;
  height: 100%;
  background-image: linear-gradient(rgba(255,255,255,.2) 1px, transparent 1px),
                    linear-gradient(90deg, rgba(255,255,255,.2) 1px, transparent 1px);
  background-size: 20px 20px;
}

@media (max-width: 768px) {
  .c-team-slider {
    height: 100vh;
  }
  
  .c-team-slider__track {
    padding-left: max(2rem, calc((100vw - 250px)));
    padding-right: max(2rem, calc((100vw - 250px)));
  }
  
  .c-team-slider__item {
    width: 250px;
    height: 375px;
  }
}

@media (max-width: 480px) {
  .c-team-slider {
    height: 100vh;
  }

  .c-team-slider__track {
    padding-left: max(1rem, calc((100vw - 200px)));
    padding-right: max(1rem, calc((100vw - 200px)));
  }

  .c-team-slider__item {
    width: 200px;
    height: 300px;
  }
}

.relative.flex > div:hover {
  z-index: 50 !important;
}

img {
  filter: brightness(0.95);
  border: 1px solid rgba(255, 255, 255, 0.1);
  max-width: 100%;
  height: auto;
}

.overflow-visible {
  overflow: visible !important;
}
</style> 