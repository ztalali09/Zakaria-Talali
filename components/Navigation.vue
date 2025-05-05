<template>
  <div 
    class="navigation-overlay"
    :class="{ 'active': isActive }"
    @mouseleave="handleMouseLeave"
  >
    <div class="navigation-content">
      <nav class="nav-links">
        <div v-for="(link, i) in links" :key="i" class="link-container">
          <div 
            class="link-wrapper"
            :style="{ 
              transitionDelay: `${0.1 * i}s`
            }"
          >
            <a 
              :href="link.href" 
              @click="handleLinkClick"
              class="nav-link"
            >
              {{ link.title.toUpperCase() }}
            </a>
          </div>
        </div>
      </nav>
      
      <div class="nav-footer">
        <div v-for="(link, i) in socialLinks" :key="i" class="social-link-wrapper">
          <a 
            :href="link.href"
            class="social-link"
            :style="{ 
              transitionDelay: `${0.3 + (0.1 * i)}s`
            }"
          >
            {{ link.title.toUpperCase() }}
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  isActive: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['close']);

const links = [
  {
    title: "Home",
    href: "#home"
  },
  {
    title: "Our Approach",
    href: "#approaches"
  },
  {
    title: "Services",
    href: "#services"
  },
  {
    title: "Expertise",
    href: "#expertise"
  }
];

const socialLinks = [
  {
    title: "LinkedIn",
    href: "#"
  },
  {
    title: "GitHub",
    href: "#"
  },
  {
    title: "Fiverr",
    href: "#"
  },
  {
    title: "Upwork",
    href: "#"
  }
];

const handleLinkClick = () => {
  emit('close');
};

const handleMouseLeave = () => {
  emit('close');
};
</script>

<style scoped lang="scss">
.navigation-overlay {
  position: fixed;
  top: 0;
  right: 0;
  width: 480px;
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  z-index: 40;
  padding: 120px 50px 50px;
  transform: translateX(100%);
  transition: transform 0.75s cubic-bezier(0.76, 0, 0.24, 1);

  &.active {
    transform: translateX(0);
  }
}

.navigation-content {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.nav-links {
  .link-container {
    overflow: hidden;
    margin-bottom: 2rem;
  }

  .link-wrapper {
    transform: translateY(100%);
    opacity: 0;
    transition: all 0.75s cubic-bezier(0.76, 0, 0.24, 1);

    .active & {
      transform: translateY(0);
      opacity: 1;
    }
  }

  .nav-link {
    font-family: 'HK Grotesk Wide', sans-serif;
    font-weight: 500;
    font-size: 2.5rem;
    color: #00FFB2;
    text-decoration: none;
    display: inline-block;
    position: relative;
    transition: all 0.3s ease;
    letter-spacing: 0.05em;

    &::before {
      content: '';
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 0;
      background: linear-gradient(90deg, transparent, #00FFB2, transparent);
      opacity: 0;
      transition: all 0.3s ease;
    }

    &::after {
      content: '';
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 2px;
      background: #00FFB2;
      transform: scaleX(0);
      transform-origin: right;
      transition: transform 0.3s ease;
    }

    &:hover {
      transform: translateX(20px);
      text-shadow: 0 0 10px rgba(0, 255, 178, 0.5);

      &::before {
        height: 100%;
        opacity: 0.1;
      }

      &::after {
        transform: scaleX(1);
        transform-origin: left;
      }
    }
  }
}

.nav-footer {
  display: flex;
  gap: 2rem;

  .social-link {
    font-family: 'HK Grotesk Wide', sans-serif;
    font-weight: 400;
    font-size: 1rem;
    color: #00FFB2;
    text-decoration: none;
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.75s cubic-bezier(0.76, 0, 0.24, 1);
    letter-spacing: 0.1em;
    position: relative;

    .active & {
      opacity: 1;
      transform: translateY(0);
    }

    &::after {
      content: '';
      position: absolute;
      left: 0;
      bottom: -2px;
      width: 100%;
      height: 1px;
      background: #00FFB2;
      transform: scaleX(0);
      transform-origin: right;
      transition: transform 0.3s ease;
    }

    &:hover {
      opacity: 1;
      text-shadow: 0 0 10px rgba(0, 255, 178, 0.5);

      &::after {
        transform: scaleX(1);
        transform-origin: left;
      }
    }
  }
}

// Responsive
@media (max-width: 768px) {
  .navigation-overlay {
    width: 100%;
    padding: 100px 30px 30px;
  }

  .nav-links .nav-link {
    font-size: 2rem;
  }
}
</style> 