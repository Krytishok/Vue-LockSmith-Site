<template>
  <div class="hamburger-menu">
    <div class="menu-header-wrapper" :class="{ 'menu-open': isOpen }">
      <div class="menu-header">
        <button
          class="hamburger-button"
          :class="{ 'is-active': isOpen }"
          @click="toggleMenu"
          aria-label="Toggle menu"
          aria-expanded="isOpen"
          aria-controls="menu-content"
        >
          <span class="hamburger-lines">
            <span class="line line-1"></span>
            <span class="line line-2"></span>
            <span class="line line-3"></span>
          </span>
        </button>
        
        <h1 class="logo-text">Guardian LockSmith</h1>
      </div>
    </div>

    <transition name="slide">
      <nav
        v-show="isOpen"
        id="menu-content"
        class="menu-content"
        @click.stop
      >
        <ul class="menu-list">
          <li class="menu-item">
            <button class="menu-button" @click="navigateTo('contact')">
              Contacts
            </button>
          </li>
          <li class="menu-item">
            <button @click="emit('navigate', 'test')" class="menu-button">
            Test
            </button>
          </li>
          <li class="menu-item">
            <button class="menu-button" @click="navigateTo('table')">
              Table
            </button>
          </li>
        </ul>
      </nav>
    </transition>

    <transition name="fade">
      <div
        v-if="isOpen"
        class="menu-overlay"
        @click="closeMenu"
      ></div>
    </transition>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

const isOpen = ref(false);
const emit = defineEmits(['toggle', 'navigate']);


const toggleMenu = () => {
  isOpen.value = !isOpen.value;
  emit('toggle', isOpen.value);
};

const closeMenu = () => {
  isOpen.value = false;
  emit('toggle', false);
};

const navigateTo = (section) => {
  emit('navigate', section);
  closeMenu();
};

const handleKeydown = (e) => {
  if (e.key === 'Escape' && isOpen.value) {
    closeMenu();
  }
};

const handleResize = () => {
  if (window.innerWidth > 768 && isOpen.value) {
    closeMenu();
  }
};

onMounted(() => {
  document.addEventListener('keydown', handleKeydown);
  window.addEventListener('resize', handleResize);
});

onBeforeUnmount(() => {
  document.removeEventListener('keydown', handleKeydown);
  window.removeEventListener('resize', handleResize);
});
</script>

<style scoped>
.hamburger-menu {
  position: relative;
  z-index: 1000;
  font-family: 'Arial', sans-serif;
}

/* Обертка для фиксированного позиционирования */
.menu-header-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1100;
}

/* Заголовок меню с адаптивной шириной */
.menu-header {
  display: inline-flex;
  align-items: center;
  padding: 12px 16px;
  background: none;
  transition: all 0.3s ease;
}

/* Улучшенный гамбургер с идеальной анимацией */
.hamburger-button {
  padding: 0;
  margin-right: 12px;
  background: none;
  border: none;
  cursor: pointer;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.hamburger-lines {
  padding: 0;
  margin: 0;
  width: 26px;
  height: 17px;
  position: relative;
}

.hamburger-lines .line {
  position: absolute;
  padding: 0;
  margin: 0;
  left: 0;
  width: 100%;
  max-width: 26px;
  height: 2px;
  background-color: white;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.line-1 {
  top: 0;
  transform-origin: left center;
}

.line-2 {
  top: 45%;
  transform-origin: left center;
}

.line-3 {
  bottom: 0;
  transform-origin: left center;
}

/* Плавная анимация в крестик */
.hamburger-button.is-active .line-1 {
  transform: translateX(2px) translateY(-2px) rotate(45deg);
}

.hamburger-button.is-active .line-2 {
  opacity: 0;
}

.hamburger-button.is-active .line-3 {
  transform: translateX(2px) translateY(2px) rotate(-45deg);
}

.logo-text {
  color: white;
  font-size: 1.25rem;
  font-weight: 600;
  margin: 0;
  position: relative;
  padding-bottom: 4px;
  line-height: 1.3;
  letter-spacing: 0.5px;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
  white-space: nowrap;
  visibility: hidden;
}

.logo-text::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 2px;
  background: #4fc3f7;
  border-radius: 1px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
}

/* Адаптивное выезжающее меню */
.menu-content {
  position: fixed;
  top: 0;
  left: 0;
  width: calc(100% - 40px);
  max-width: 320px;
  height: 100vh;
  background: #2c3e50;
  box-shadow: 2px 0 15px rgba(0, 0, 0, 0.1);
  z-index: 1050;
  padding-top: 70px;
  overflow-y: auto;
}

.menu-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.menu-item {
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.menu-button {
  width: 100%;
  padding: 16px 24px;
  text-align: left;
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.menu-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

/* Оверлей */
.menu-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1040;
  backdrop-filter: blur(2px);
}

/* Анимации */
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.slide-enter-from,
.slide-leave-to {
  transform: translateX(-100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Адаптивные стили */
@media (min-width: 768px) {
  .menu-header {
    padding: 16px 24px;
    border-radius: 0 0 12px 0;
  }
  
  .logo-text {
    font-size: 1rem;
    visibility: visible;
  }
  
  .menu-content {
    max-width: 350px;
    padding-top: 80px;
  }
  
  .menu-button {
    padding: 18px 28px;
    font-size: 17px;
  }
}
</style>