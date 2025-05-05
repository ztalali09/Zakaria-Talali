<template>
  <section class="hero-section relative w-full min-h-screen overflow-hidden">
    <!-- Interactive Background with Three.js -->
    <div class="hero-section__background absolute inset-0 z-0">
      <canvas 
        ref="canvas" 
        class="interactive-background__canvas absolute inset-0 w-full h-full" 
        data-engine="three.js r155"
      ></canvas>
    </div>

    <!-- Tailwind Grid Overlay -->
    <div class="absolute inset-0 z-10 bg-[url('/grid.svg')] bg-repeat opacity-0"></div>
    
    <!-- Content Container -->
    <div class="relative z-20 container mx-auto px-4 sm:px-6 lg:px-8 h-screen flex items-center justify-center">
      <div class="text-center w-full max-w-6xl mx-auto">
        <!-- Logo -->
        <div class="mb-4 sm:mb-6 md:mb-8">
          <h3 class="text-2xl sm:text-3xl md:text-4xl font-light text-[#00FFB2]/90">
            Based in Morocco
          </h3>
        </div>
        
        <!-- Main Heading -->
        <div class="mb-8 sm:mb-10 md:mb-12">
          <h1 class="text-5xl sm:text-6xl md:text-7xl lg:text-8xl xl:text-9xl 2xl:text-[10rem] font-bold text-[#00FFB2]/95 leading-tight sm:leading-none">
            <span class="block animate-fadeInUp">ZAKARIA</span>
            <span class="block animate-fadeInUp">PORTFOLIO</span>
          </h1>
        </div>
        
        <!-- Subheading -->
        <div class="mb-8 sm:mb-10 md:mb-12">
          <p class="text-base sm:text-lg md:text-xl lg:text-2xl text-[#00FFB2]/80 max-w-2xl mx-auto px-4">
            Exploring the intersection of design and technology. Browse my latest projects and experiments.
          </p>
        </div>

        <!-- Explore Button -->
        <div 
          v-if="!isScrollUnlocked && showFloatingButton"
          class="fixed cursor-pointer"
          :style="{
            left: mousePosition.x + 'px',
            top: mousePosition.y + 'px',
            transform: 'translate(-50%, -50%)',
            transition: 'none',
            opacity: showFloatingButton ? 1 : 0,
            transition: 'opacity 0.3s ease-out'
          }"
        >
          <button 
            @click="scrollToNextSection" 
            class="px-8 py-4 bg-[#00FFB2]/10 backdrop-blur-sm rounded-full text-[#00FFB2] text-xl hover:bg-[#00FFB2]/20 transition-all duration-300 transform hover:scale-105 animate-pulse"
          >
            Discover
          </button>
        </div>

        <!-- Static Button (shown after scroll is unlocked) -->
        <div v-else class="mt-12">
          <button 
            @click="scrollToNextSection"
            class="px-8 py-4 bg-[#00FFB2]/10 backdrop-blur-sm rounded-full text-[#00FFB2] text-xl hover:bg-[#00FFB2]/20 transition-all duration-300 transform hover:scale-105"
          >
            Discover
          </button>
        </div>
      </div>
    </div>
    
    <!-- Menu Button -->
    <div 
      class="fixed top-4 sm:top-6 md:top-8 right-4 sm:right-6 md:right-8 z-30 flex items-center"
      @mouseenter="handleMenuHover(true)"
      @mouseleave="handleMenuHover(false)"
    >
      <button 
        class="hidden sm:block px-6 md:px-8 py-2 md:py-3 rounded-full border-2 border-[#00FFB2]/90 text-[#00FFB2]/90 hover:bg-[#00FFB2] hover:text-black transition-all duration-300 mr-4 backdrop-blur-sm bg-black/20"
        @mouseenter="handleMenuHover(true)"
        @mouseleave="handleMenuHover(false)"
      >
        Get In Touch
      </button>
      <button 
        class="w-10 h-10 sm:w-12 sm:h-12 rounded-full border-2 border-[#00FFB2]/90 flex items-center justify-center text-[#00FFB2]/90 hover:bg-[#00FFB2] hover:text-black transition-all duration-300 backdrop-blur-sm bg-black/20"
        @mouseenter="handleMenuHover(true)"
        @mouseleave="handleMenuHover(false)"
        @click="toggleMenu"
      >
        <span class="sr-only">Menu</span>
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-menu"><line x1="4" x2="20" y1="12" y2="12"/><line x1="4" x2="20" y1="6" y2="6"/><line x1="4" x2="20" y1="18" y2="18"/></svg>
      </button>
    </div>

    <!-- Navigation Menu -->
    <Navigation :is-active="isMenuOpen" @close="closeMenu" />
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { useNuxtApp } from '#app';
import Navigation from './Navigation.vue';

// Canvas reference
const canvas = ref(null);
const { $loading } = useNuxtApp();

// Mouse position tracking
const mousePosition = ref({ x: 0, y: 0 });
const isScrollUnlocked = ref(false);
const isOverMenu = ref(false);
const showFloatingButton = ref(true);

// Add these new refs and methods
const isMenuOpen = ref(false);

// Update mouse position
const updateMousePosition = (event) => {
  if (!isScrollUnlocked.value && !isOverMenu.value && showFloatingButton.value) {
    mousePosition.value = {
      x: event.clientX,
      y: event.clientY
    };
  }
};

// Handle menu hover
const handleMenuHover = (isHovering) => {
  isOverMenu.value = isHovering;
  showFloatingButton.value = !isHovering;
};

// Function to scroll to the next section
const scrollToNextSection = () => {
  const approachSection = document.querySelector('.approach-section');
  if (approachSection) {
    approachSection.scrollIntoView({ 
      behavior: 'smooth',
      block: 'start'
    });
  }
  isScrollUnlocked.value = true;
  document.body.style.overflow = 'auto';
  document.body.style.height = 'auto';
};

// Replace the unlockScroll function with scrollToNextSection
const unlockScroll = scrollToNextSection;

// Three.js variables
let scene, camera, renderer, uniforms, animationFrameId;

// Initialize Three.js scene
const initThree = () => {
  if (!canvas.value) return;
  
  // Import Three.js dynamically (for Nuxt compatibility)
  import('three').then((THREE) => {
    // Set up renderer
    renderer = new THREE.WebGLRenderer({ 
      canvas: canvas.value,
      alpha: true,
      antialias: true
    });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setPixelRatio(window.devicePixelRatio);
    
    // Set up scene
    scene = new THREE.Scene();
    
    // Set up camera
    camera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0, 1);
    
    // Create shader material with purple gradient
    uniforms = {
      u_time: { value: 0 },
      u_resolution: { value: new THREE.Vector2(window.innerWidth, window.innerHeight) },
      u_mouse: { value: new THREE.Vector2(0.5, 0.5) },
      u_mouseIntensity: { value: 0.0 },
      u_color1: { value: new THREE.Color('#00FFB2') },  // Vert menthe clair
      u_color2: { value: new THREE.Color('#00D1A0') },  // Vert menthe
      u_color3: { value: new THREE.Color('#00A88C') },  // Vert émeraude
      u_color4: { value: new THREE.Color('#008B74') }   // Vert profond
    };
    
    const fragmentShader = `
      uniform float u_time;
      uniform vec2 u_resolution;
      uniform vec2 u_mouse;
      uniform float u_mouseIntensity;
      uniform vec3 u_color1;
      uniform vec3 u_color2;
      uniform vec3 u_color3;
      uniform vec3 u_color4;
      
      varying vec2 vUv;
      
      void main() {
        // Create animated gradient with faster movement and mouse interaction
        float distanceFromMouse = length(vUv - u_mouse);
        float mouseEffect = (1.0 - distanceFromMouse) * u_mouseIntensity;
        
        float noise = sin(vUv.x * 8.0 + u_time + mouseEffect * 5.0) * 0.1 + 
                     cos(vUv.y * 6.0 + u_time * 1.2 + mouseEffect * 5.0) * 0.1;
        
        // Base gradient from top-left to bottom-right with mouse influence
        vec2 gradPos = vUv + vec2(
          sin(u_time * 0.3 + mouseEffect) * 0.1, 
          cos(u_time * 0.4 + mouseEffect) * 0.1
        );
        float gradVal = (gradPos.x + gradPos.y) * 0.5 + noise;
        
        // Mix colors based on gradient value
        vec3 color;
        if (gradVal < 0.33) {
          color = mix(u_color1, u_color2, smoothstep(0.0, 0.33, gradVal));
        } else if (gradVal < 0.66) {
          color = mix(u_color2, u_color3, smoothstep(0.33, 0.66, gradVal));
        } else {
          color = mix(u_color3, u_color4, smoothstep(0.66, 1.0, gradVal));
        }
        
        gl_FragColor = vec4(color, 1.0);
      }
    `;
    
    const vertexShader = `
      varying vec2 vUv;
      
      void main() {
        vUv = uv;
        gl_Position = vec4(position, 1.0);
      }
    `;
    
    const material = new THREE.ShaderMaterial({
      uniforms: uniforms,
      vertexShader: vertexShader,
      fragmentShader: fragmentShader
    });
    
    const geometry = new THREE.PlaneGeometry(2, 2);
    const mesh = new THREE.Mesh(geometry, material);
    scene.add(mesh);
    
    window.addEventListener('resize', onResize);
    animate();
    
    // Signal that Three.js is ready
    $loading.componentLoaded('HeroSection');
  }).catch(error => {
    console.error('Failed to load Three.js:', error);
    // Still mark component as loaded on error
    $loading.componentLoaded('HeroSection');
  });
};

// Animation loop
const animate = () => {
  uniforms.u_time.value += 0.015;
  
  renderer.render(scene, camera);
  animationFrameId = requestAnimationFrame(animate);
};

// Handle window resize
const onResize = () => {
  if (!renderer || !canvas.value) return;
  
  const width = canvas.value.clientWidth;
  const height = canvas.value.clientHeight;
  
  renderer.setSize(width, height);
  uniforms.u_resolution.value.set(width, height);
};

// Handle mouse movement for interactive effect
const onMouseMove = (event) => {
  if (!uniforms) return;
  
  const x = event.clientX / window.innerWidth;
  const y = 1.0 - (event.clientY / window.innerHeight);
  
  uniforms.u_mouse.value.set(x, y);
  uniforms.u_mouseIntensity.value = 0.5;
};

// Handle mouse leave
const onMouseLeave = () => {
  if (!uniforms) return;
  uniforms.u_mouseIntensity.value = 0.0;
};

// Toggle menu
const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
  if (isMenuOpen.value) {
    document.body.style.overflow = 'hidden';
  } else {
    document.body.style.overflow = 'auto';
  }
};

const closeMenu = () => {
  isMenuOpen.value = false;
  document.body.style.overflow = 'auto';
};

// Lifecycle hooks
onMounted(() => {
  // Lock scroll on mount
  document.body.style.overflow = 'hidden';
  document.body.style.height = '100vh';
  
  // Add mouse move listener
  window.addEventListener('mousemove', updateMousePosition);
  
  initThree();
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mouseleave', onMouseLeave);
});

onUnmounted(() => {
  // Remove mouse move listener
  window.removeEventListener('mousemove', updateMousePosition);
  
  // Unlock scroll on unmount
  document.body.style.overflow = 'auto';
  document.body.style.height = 'auto';
  
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId);
  }
  
  window.removeEventListener('resize', onResize);
  window.removeEventListener('mousemove', onMouseMove);
  window.removeEventListener('mouseleave', onMouseLeave);
  
  if (renderer) {
    renderer.dispose();
  }
  
  if (scene) {
    scene.clear();
  }
});
</script>

<style scoped>
/* Add Font Face Rules */
@font-face {
  font-family: 'HK Grotesk Wide';
  src: url('/fonts/HKGroteskWide-Light.otf') format('opentype');
  font-weight: 300;
  font-style: normal;
}

@font-face {
  font-family: 'HK Grotesk Wide';
  src: url('/fonts/HKGroteskWide-Regular.otf') format('opentype');
  font-weight: 400;
  font-style: normal;
}

@font-face {
  font-family: 'HK Grotesk Wide';
  src: url('/fonts/HKGroteskWide-Medium.otf') format('opentype');
  font-weight: 500;
  font-style: normal;
}

@font-face {
  font-family: 'HK Grotesk Wide';
  src: url('/fonts/HKGroteskWide-SemiBold.otf') format('opentype');
  font-weight: 600;
  font-style: normal;
}

@font-face {
  font-family: 'HK Grotesk Wide';
  src: url('/fonts/HKGroteskWide-Bold.otf') format('opentype');
  font-weight: 700;
  font-style: normal;
}

@font-face {
  font-family: 'HK Grotesk Wide';
  src: url('/fonts/HKGroteskWide-ExtraBold.otf') format('opentype');
  font-weight: 800;
  font-style: normal;
}

@font-face {
  font-family: 'HK Grotesk Wide';
  src: url('/fonts/HKGroteskWide-Black.otf') format('opentype');
  font-weight: 900;
  font-style: normal;
}

@font-face {
  font-family: 'Dirtyline';
  src: url('/fonts/Dirtyline 36daysoftype 2022.woff') format('woff');
  font-weight: normal;
  font-style: normal;
}

.hero-section {
  position: relative;
  width: 100%;
  height: 100vh;
  color: #00FFB2;
  display: flex;
  align-items: center;
  background-color: #000000; /* Fond noir */
  /* Apply base font */
  font-family: 'HK Grotesk Wide', sans-serif; 
}

/* Apply Specific Fonts and Weights */
h3 {
  font-family: 'HK Grotesk Wide', sans-serif;
  font-weight: 300; /* Light */
}

h1 {
  /* Option 1: HK Grotesk Wide Bold/ExtraBold */
  font-family: 'HK Grotesk Wide', sans-serif; 
  font-weight: 800; /* ExtraBold */

  /* Option 2: Dirtyline (Uncomment to try) */
  /* font-family: 'Dirtyline', cursive; */
  /* font-weight: normal; */
}

p {
  font-family: 'HK Grotesk Wide', sans-serif;
  font-weight: 400; /* Regular */
}

button {
  font-family: 'HK Grotesk Wide', sans-serif;
  font-weight: 600; /* SemiBold */
}

.hero-section__background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

.interactive-background__canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100% !important;
  height: 100% !important;
}

/* Animation for text elements */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(100px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fadeInUp {
  animation: fadeInUp 1.2s cubic-bezier(0.16, 1, 0.3, 1) forwards;
  opacity: 0;
}

.animate-fadeInUp:nth-child(1) {
  animation-delay: 0.2s;
}

.animate-fadeInUp:nth-child(2) {
  animation-delay: 0.4s;
}

/* Responsive text adjustments */
@media (max-width: 640px) {
  h1 {
    line-height: 1.2;
  }
  
  h1 span {
    margin-bottom: 0.5rem;
  }
}

/* Add styles for the explore button */
.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.7;
  }
}

/* Add styles for the floating button */
.fixed {
  position: fixed;
  z-index: 50;
  pointer-events: auto;
  will-change: transform;
}

/* Ensure the button is always visible */
button {
  pointer-events: auto;
}
</style> 