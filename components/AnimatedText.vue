<template>
  <div ref="container" class="animated-text">
    <p class="text-paragraph">
      <span v-for="(word, wordIndex) in words" :key="wordIndex" class="word">
        <span v-for="(char, charIndex) in word" :key="`${wordIndex}-${charIndex}`" class="char">
          <span class="shadow">{{ char }}</span>
          <span 
            v-motion
            :initial="{ opacity: 0, color: '#ffffff' }"
            :enter="{ opacity: 1, color: '#00FFB2' }"
            :visible="{ opacity: 1, color: '#00FFB2' }"
            :visibleOnce="{ opacity: 1, color: '#00FFB2' }"
            :style="{ 
              opacity: getCharOpacity(wordIndex, charIndex),
              color: getCharColor(wordIndex, charIndex)
            }"
            class="char-content"
          >
            {{ char }}
          </span>
        </span>
        <span class="space">&nbsp;</span>
      </span>
    </p>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useScroll } from '@vueuse/core'

const props = defineProps({
  text: {
    type: String,
    required: true
  }
})

const container = ref(null)
const words = computed(() => props.text.split(' '))

const { y } = useScroll(container)

const getCharOpacity = (wordIndex, charIndex) => {
  const wordStart = wordIndex / words.value.length
  const wordEnd = wordStart + (1 / words.value.length)
  const charStep = (wordEnd - wordStart) / words.value[wordIndex].length
  const charStart = wordStart + (charIndex * charStep)
  const charEnd = charStart + charStep

  // Calcul de la progression centrée sur l'écran
  const viewportCenter = window.innerHeight / 2
  const elementTop = container.value?.offsetTop || 0
  const elementCenter = elementTop + (container.value?.offsetHeight || 0) / 2
  const distanceFromCenter = Math.abs(y.value + viewportCenter - elementCenter)
  const maxDistance = window.innerHeight / 2
  const progress = 1 - (distanceFromCenter / maxDistance)

  return Math.min(Math.max((progress - charStart) / (charEnd - charStart), 0), 1)
}

const getCharColor = (wordIndex, charIndex) => {
  const opacity = getCharOpacity(wordIndex, charIndex)
  // Interpolation entre blanc (#FFFFFF) et vert (#00FFB2)
  const r = Math.round(255 * (1 - opacity) + 0 * opacity)
  const g = Math.round(255 * (1 - opacity) + 255 * opacity)
  const b = Math.round(255 * (1 - opacity) + 178 * opacity)
  return `rgb(${r}, ${g}, ${b})`
}
</script>

<style scoped>
.animated-text {
  position: relative;
}

.text-paragraph {
  position: relative;
  line-height: 1.2;
}

.word {
  display: inline-block;
  position: relative;
}

.char {
  position: relative;
  display: inline-block;
}

.shadow {
  position: absolute;
  opacity: 0.2;
}

.char-content {
  position: relative;
  transition: all 0.3s ease;
}

.space {
  display: inline-block;
  width: 0.25em;
}
</style> 