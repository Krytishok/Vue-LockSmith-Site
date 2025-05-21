<template>
  <div class="app">
    <!-- Фоновые слои -->
    <div 
      class="background-layer" 
      v-for="(bg, index) in visibleBackgrounds" 
      :key="'bg-'+index"
      :style="{ background: bg.style, zIndex: -index }"
      :class="{ active: bg.active }"
    ></div>

    <!-- Основной контент -->
    <div class="main-content-container">
      <CornerIcons class="corner-icons" />
      <HamburgerMenu 
        @toggle="handleMenuToggle"
        @navigate="handleNavigation"
      />
      <div class="content" :class="{ 'menu-pushed': isMenuOpen }">
        <div class="content-wrapper">
          <MainContent class="main-content" />
          <ContactForm class="contact-form" />
          <div class="background-controls">
            <button 
            v-for="(bg, index) in backgrounds" 
            :key="index"
            :class="{ active: currentIndex === index }"
            @click="changeBackground(index)"
            :style="{ backgroundImage: `url(${bg.image})` }"
            class="bg-btn">
            </button>
      </div>
        </div>
      </div>
      <LockSmithQuiz ref="quiz" />
      <Table ref="table" />
      
      <!-- Кнопки-слайдеры для смены фона -->
      
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed } from 'vue';
import HamburgerMenu from './components/HamburgerMenu.vue';
import MainContent from './components/MainContent.vue';
import ContactForm from './components/ContactForm.vue';
import CornerIcons from './components/CornerIcons.vue';
import LockSmithQuiz from './components/LockSmithQuiz.vue';
import Table from './components/Table.vue';

const isMenuOpen = ref(false);
const quiz = ref(null);
const currentIndex = ref(0);
let intervalId = null;
const table = ref(null);

const backgrounds = [
  {
    image: 'https://c.pxhere.com/photos/30/58/keys_key_lock_door_symbols_access_carbine_locked-1358293.jpg!d',
    style: 'linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.6)), url(https://c.pxhere.com/photos/30/58/keys_key_lock_door_symbols_access_carbine_locked-1358293.jpg!d) center/cover fixed'
  },
  {
    image: 'https://www.diamondlocksmiths.com.au/wp-content/uploads/2023/08/locksmith-repairing-door-lock.jpg',
    style: 'linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.6)), url(https://www.diamondlocksmiths.com.au/wp-content/uploads/2023/08/locksmith-repairing-door-lock.jpg) center/cover fixed'
  },
  {
    image: 'https://images.squarespace-cdn.com/content/v1/5ba1587df93fd4082124dc1c/1537990975860-0WQVG5KOTEO7LPUOB167/safe+unlock+services',
    style: 'linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.6)), url(https://images.squarespace-cdn.com/content/v1/5ba1587df93fd4082124dc1c/1537990975860-0WQVG5KOTEO7LPUOB167/safe+unlock+services) center/cover fixed'
  },
  {
    image: 'https://bodeandbode.com/wp-content/uploads/sites/49/2019/05/car-locksmith.jpeg',
    style: 'linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.6)), url(https://bodeandbode.com/wp-content/uploads/sites/49/2019/05/car-locksmith.jpeg) center/cover fixed'
  }
];

const visibleBackgrounds = computed(() => {
  return backgrounds.map((bg, index) => ({
    ...bg,
    active: index === currentIndex.value
  }));
});

const changeBackground = (index) => {
  currentIndex.value = index;
  resetInterval();
};

const nextBackground = () => {
  currentIndex.value = (currentIndex.value + 1) % backgrounds.length;
};

const startInterval = () => {
  intervalId = setInterval(nextBackground, 15000);
};

const resetInterval = () => {
  clearInterval(intervalId);
  startInterval();
};

onMounted(() => {
  startInterval();
});

onBeforeUnmount(() => {
  clearInterval(intervalId);
});

const handleMenuToggle = (isOpen) => {
  isMenuOpen.value = isOpen;
};

const handleNavigation = (section) => {
  const element = document.getElementById(section);
  if (section === 'test') {
    quiz.value.showQuiz = true;
    isMenuOpen.value = false;
  } else if (section === 'table') {
    table.value.showTable = true; 
    isMenuOpen.value = false;
  } else if (element) {
    element.scrollIntoView({ behavior: 'smooth' });
  } 
};
</script>

<style scoped>
.app {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Стили для фоновых слоев */
.background-layer {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-size: cover;
  background-position: center;
  opacity: 0;
  transition: opacity 1s ease-in-out;
  z-index: -1;
}

.background-layer.active {
  opacity: 1;
}

.main-content-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  position: relative;
}

.content {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: transform 0.3s ease;
  min-height: 100%;
}

.content-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  max-width: 1200px;
  width: 100%;
  padding: 20px;
  margin: auto;
}

.main-content {
  position: relative;
  left: -5%;
  flex: 1;
  max-width: 600px;
}

.contact-form {
  background: none;
  box-shadow: none;
  position: relative;
  left: 5%;
  flex: 1;
  max-width: 500px;
}

.menu-pushed {
  transform: translateX(280px);
}

.corner-icons {
  z-index: 1000;
  position: fixed;
  top: 0;
  right: 0;
}

/* Стили для кнопок-слайдеров */
.background-controls {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
  z-index: 100;
}

.bg-btn {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid white;
  cursor: pointer;
  background-size: cover;
  background-position: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.bg-btn:hover {
  transform: scale(1.1);
}

.bg-btn.active {
  transform: scale(1.2);
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.8);
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
    gap: 15px;
  }
  
  .main-content,
  .contact-form {
    left: 0;
    max-width: 100%;
    width: 100%;
  }

  .main-content{
    padding-left: 0px;
  }



  .background-controls {
    position: static;
    transform: none;
    justify-content: center;
    margin-top: 5px;
    padding-bottom: 20px;
  }

  .bg-btn {
    width: 30px;
    height: 30px;
  }
}
</style>