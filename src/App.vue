<template>
  <div class="app">
    <HamburgerMenu 
      @toggle="handleMenuToggle"
      @navigate="handleNavigation"
    />
    <div class="content" :class="{ 'menu-pushed': isMenuOpen }">
      <!-- Основной контент всегда виден -->
       <MainContent />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import HamburgerMenu from './components/HamburgerMenu.vue';
import MainContent from './components/MainContent.vue';
import ContactForm from './components/ContactForm.vue';

const isMenuOpen = ref(false);

const handleMenuToggle = (isOpen) => {
  isMenuOpen.value = isOpen;
};

const handleNavigation = (section) => {
  const element = document.getElementById(section);
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' });
  }
};
</script>

<style scoped>
.app {
  position: relative;
  min-height: 100vh;
  overflow-x: hidden;
}

.content {
  transition: transform 0.3s ease;
}

.menu-pushed {
  transform: translateX(280px);
}



@media (min-width: 769px) {
  .menu-pushed {
    transform: translateX(320px);
  }
}
</style>