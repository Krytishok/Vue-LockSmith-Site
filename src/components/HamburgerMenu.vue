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
            <div class="dropdown-wrapper">
              <button 
                class="menu-button dropdown-button" 
                @click="toggleContactsDropdown"
                :aria-expanded="contactsDropdownOpen"
              >
                Contacts
                <span class="dropdown-arrow" :class="{ 'rotate': contactsDropdownOpen }">›</span>
              </button>
              <transition name="dropdown">
                <ul v-show="contactsDropdownOpen" class="dropdown-menu">
                  <li>
                    <button class="dropdown-item" @click="navigateTo('contact-email')">Email</button>
                  </li>
                  <li>
                    <button class="dropdown-item" @click="navigateTo('contact-phone')">Phone</button>
                  </li>
                  <li>
                    <button class="dropdown-item" @click="navigateTo('contact-address')">Address</button>
                  </li>
                </ul>
              </transition>
            </div>
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
const contactsDropdownOpen = ref(false);
const emit = defineEmits(['toggle', 'navigate']);

const toggleMenu = () => {
  isOpen.value = !isOpen.value;
  emit('toggle', isOpen.value);
  if (!isOpen.value) {
    contactsDropdownOpen.value = false;
  }
};

const closeMenu = () => {
  isOpen.value = false;
  contactsDropdownOpen.value = false;
  emit('toggle', false);
};

const toggleContactsDropdown = () => {
  contactsDropdownOpen.value = !contactsDropdownOpen.value;
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


.menu-header-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1100;
}


.menu-header {
  display: inline-flex;
  align-items: center;
  padding: 12px 16px;
  background: none;
  transition: all 0.3s ease;
}


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
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease, visibility 0.3s ease;
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
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.menu-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

/* Dropdown styles */
.dropdown-wrapper {
  position: relative;
}

.dropdown-button {
  position: relative;
}

.dropdown-arrow {
  transition: transform 0.3s ease;
  font-size: 1.2em;
  margin-left: 8px;
}

.dropdown-arrow.rotate {
  transform: rotate(90deg);
}

.dropdown-menu {
  list-style: none;
  padding: 0;
  margin: 0;
  background: rgba(0, 0, 0, 0.2);
  overflow: hidden;
}

.dropdown-item {
  width: 100%;
  padding: 12px 24px 12px 36px;
  text-align: left;
  background: none;
  border: none;
  color: white;
  font-size: 14px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.dropdown-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

/* Dropdown animation */
.dropdown-enter-active,
.dropdown-leave-active {
  transition: all 0.3s ease;
  max-height: 200px;
}

.dropdown-enter-from,
.dropdown-leave-to {
  opacity: 0;
  max-height: 0;
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

.menu-open .logo-text {
  opacity: 1;
  visibility: visible;
}

/* Адаптивные стили */
@media (min-width: 768px) {
  .menu-header {
    padding: 16px 24px;
    border-radius: 0 0 12px 0;
  }
  
  .logo-text {
    opacity: 1;
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
  
  .dropdown-item {
    padding: 14px 28px 14px 42px;
    font-size: 15px;
  }
}
</style>