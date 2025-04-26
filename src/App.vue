<template>
  <div class="app">
    <HamburgerMenu :isOpen="isMenuOpen" @toggle="toggleMenu" />
    <div class="content" :class="{ 'menu-open': isMenuOpen }">
      <MainContent />
      <ContactForm />
    </div>    
  </div>
  <div class="anketa">
    <Questionnaire />
  </div>
  <Table />

</template>

<script setup>
import { ref } from 'vue';
import HamburgerMenu from './components/HamburgerMenu.vue';
import MainContent from './components/MainContent.vue';
import ContactForm from './components/ContactForm.vue';
import Table from './components/Table.vue';
import Questionnaire from './components/Questionnaire.vue';

const isMenuOpen = ref(false);

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};
</script>

<style scoped>
.app {
  display: flex;
  min-height: 100vh;
  position: relative;
  overflow-x: hidden;
}

.content {
  display: flex;
  width: 100%;
  transition: transform 0.3s ease;
}

.menu-open {
  transform: translateY(250px);
}

@media (max-width: 768px) {
  .content {
    flex-direction: column;
  }
  
  .menu-open {
    transform: translateY(300px);
  }
}
</style>