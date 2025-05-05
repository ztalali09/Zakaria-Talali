<template>
  <section class="approach-section relative w-full min-h-screen overflow-hidden bg-[#0A0A0A] rounded-t-[2.5rem] -mt-8">
    <!-- Content Container -->
    <div class="relative z-20 container mx-auto px-4 py-12">
      <!-- Section Number and Title -->
      <div class="pt-4 md:pt-1 max-w-7xl mx-auto">
        <div class="border-section">
          <h3 class="text-base md:text-lg font-light text-white tracking-wider">
            02/ <span class="ml-2">Our approach</span>
          </h3>
          <div class="border-bottom mt-4"></div>
        </div>
      </div>
      
      <!-- Main Content -->
      <div class="mt-20 mb-32">
        <div class="w-full max-w-7xl mx-auto">
        <!-- Main Heading -->
          <div class="text-left mb-32">
            <AnimatedText 
              text="Delivering tailored digital solutions to meet the unique needs of our clients"
              class="text-4xl sm:text-5xl md:text-6xl lg:text-[72px] font-normal text-white leading-[1.15] tracking-normal"
            />
          </div>

          <!-- Services Grid -->
          <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
            <!-- Web Design & E-commerce Card -->
            <div class="service-card group relative p-16 border border-white/10 rounded-2xl overflow-hidden min-h-[400px] cursor-pointer">
              <div class="absolute inset-0 bg-[#00FFB2]/0 transition-colors duration-500 group-hover:bg-[#104911]"></div>
              <div class="grid-pattern absolute inset-0 opacity-20"></div>
              <div class="relative z-10">
                <!-- Icon -->
                <div class="mb-12 transition-colors duration-500 flex justify-center">
                  <img src="/iconshop.png" alt="Shop Icon" class="w-28 h-28 object-contain shop-animation" />
                </div>
                <!-- Title -->
                <h4 class="text-4xl font-medium text-white mb-6 transition-colors duration-500 group-hover:text-black">Web Design & E-commerce</h4>
                <!-- Description -->
                <p class="text-white/70 text-xl leading-relaxed transition-colors duration-500 group-hover:text-black/90">Creating visually stunning websites and Shopify stores with custom designs, seamless user experiences, and optimized performance to elevate your brand online.</p>
              </div>
            </div>

            <!-- Web & App Development Card -->
            <div class="service-card group relative p-16 border border-white/10 rounded-2xl overflow-hidden min-h-[400px] cursor-pointer">
              <div class="absolute inset-0 bg-[#00FFB2]/0 transition-colors duration-500 group-hover:bg-[#a8d5e2]"></div>
              <div class="grid-pattern absolute inset-0 opacity-20"></div>
              <div class="relative z-10">
                <!-- Icon -->
                <div class="mb-12 transition-colors duration-500 flex justify-center">
                  <img src="/laptopicon.png" alt="Laptop Icon" class="w-28 h-28 object-contain laptop-animation" />
                </div>
                <!-- Title -->
                <h4 class="text-4xl font-medium text-white mb-6 transition-colors duration-500 group-hover:text-black">Web & App Development</h4>
                <!-- Description -->
                <p class="text-white/70 text-xl leading-relaxed transition-colors duration-500 group-hover:text-black/90">Developing responsive websites and dynamic apps using HTML, CSS, JavaScript, Vue.js, Next.js, and Java, tailored to your unique vision and goals.</p>
              </div>
            </div>
        </div>
        
        <!-- CTA Button -->
          <div class="mt-20">
          <NuxtLink 
            to="/approach"
              class="cta-button inline-flex items-center text-base text-white border border-white/20 rounded-full px-8 py-4 relative overflow-hidden group"
          >
              <span class="relative z-10 mr-2 group-hover:text-black transition-colors duration-500">Learn more about our approach</span>
              <span class="relative z-10 w-2 h-2 rounded-full bg-[#FF4D00] ml-1 group-hover:bg-black transition-colors duration-500"></span>
          </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import AnimatedText from './AnimatedText.vue'
</script>

<style scoped>
.approach-section {
  position: relative;
  width: 100%;
  min-height: 100vh;
  color: white;
  font-family: 'HK Grotesk', sans-serif;
  margin-top: -2rem;
}

.border-section {
  position: relative;
}

.border-bottom {
  width: 100%;
  height: 1px;
  background-color: rgba(255, 255, 255, 0.2);
}

.service-card {
  background-color: rgba(255, 255, 255, 0.02);
  backdrop-filter: blur(10px);
  transition: all 0.5s ease;
  position: relative;
  overflow: hidden;
}

.service-card:hover {
  border-color: transparent;
  transform: translateY(-5px);
}

.service-card:hover .grid-pattern {
  opacity: 0.5;
}

h2 {
  font-family: 'HK Grotesk', sans-serif;
  font-weight: 400;
  color: white;
}

h3, h4 {
  font-family: 'HK Grotesk', sans-serif;
  font-weight: 300;
  color: white;
}

.cta-button {
  transition: all 0.5s ease;
  color: white;
}

.cta-button::before {
  content: '';
  position: absolute;
  inset: 0;
  background: #00FFB2;
  opacity: 0;
  transition: opacity 0.5s ease;
  z-index: 1;
}

.cta-button:hover::before {
  opacity: 1;
}

.cta-button:hover {
  border-color: transparent;
}

/* Responsive adjustments */
@media (max-width: 640px) {
  h2 {
    line-height: 1.15;
    font-size: 2.5rem;
  }
  
  .approach-section {
    border-radius: 1.5rem 1.5rem 0 0;
  }
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-15px);
  }
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
}

.laptop-animation {
  animation: float 2s ease-in-out infinite;
}

.shop-animation {
  animation: pulse 2s ease-in-out infinite;
}

.grid-pattern {
  background-image: 
    linear-gradient(rgba(255, 255, 255, 0.15) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.15) 1px, transparent 1px);
  background-size: 20px 20px;
  background-position: center center;
  transition: opacity 0.5s ease;
}
</style> 