<template>
  <div class="app">
    <HamburgerMenu 
      @toggle="handleMenuToggle"
      @navigate="handleNavigation"
    />
    <div class="content" :class="{ 'menu-pushed': isMenuOpen }">
      <div class="content-wrapper">
        <MainContent class="main-content" />
        <ContactForm class="contact-form" />
      </div>
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
  display: flex;
  position: relative;
  min-height: 100vh;
  overflow-x: hidden;
}

.content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: transform 0.3s ease;
}

.content-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px; /* Уменьшили расстояние между блоками */
  max-width: 1200px;
  width: 100%;
  padding: 20px;
}

.main-content {
  position: relative;
  left: -5%; /* Уменьшили смещение влево */
  flex: 1;
  max-width: 600px;
}

.contact-form {
  background: none;
  box-shadow: none;
  position: relative;
  left: 5%; /* Уменьшили смещение вправо */
  flex: 1;
  max-width: 500px;
}

.menu-pushed {
  transform: translateX(280px);
}

@media (min-width: 769px) {
  .menu-pushed {
    transform: translateX(320px);
  }
}

@media (max-width: 768px) {
  .content-wrapper {
    flex-direction: column;
    justify-content: center;
    gap: 15px; /* Уменьшили расстояние для мобильных */
  }
  
  .main-content,
  .contact-form {
    left: 0;
    max-width: 100%;
    width: 100%;
  }
}
</style>