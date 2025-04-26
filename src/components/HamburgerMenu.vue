<template>
    <div class="hamburger-menu">
      <button class="hamburger-button" @click="$emit('toggle')">
        <span class="bar" :class="{ 'bar-1': isOpen }"></span>
        <span class="bar" :class="{ 'bar-2': isOpen }"></span>
        <span class="bar" :class="{ 'bar-3': isOpen }"></span>
      </button>
      
      <div class="menu" :class="{ 'open': isOpen }">
        <div class="menu-item">
          <img src="https://icons.veryicon.com/png/o/miscellaneous/two-color-webpage-small-icon/right-arrow-37.png" alt="Arrow" class="icon" />
          <span>Quick Response</span>
        </div>
        <div class="menu-item">
          <img src="https://static-00.iconduck.com/assets.00/email-icon-2048x2048-iejnvv9u.png" alt="Email" class="icon" />
          <span>contact@locksmith24.com</span>
        </div>
        <div class="menu-item">
          <img src="https://static-00.iconduck.com/assets.00/phone-icon-1024x1024-bn4mirqi.png" alt="Phone" class="icon" />
          <span>+1 (555) 123-4567</span>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  defineProps({
    isOpen: Boolean
  });
  
  defineEmits(['toggle']);
  </script>
  
  <style scoped>
  .hamburger-menu {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 1000;
  }
  
  .hamburger-button {
    position: absolute;
    top: 20px;
    left: 20px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 10px;
    z-index: 1001;
  }
  
  .bar {
    display: block;
    width: 25px;
    height: 3px;
    margin: 5px 0;
    background-color: #ffffff;
    transition: all 0.3s ease;
  }
  

  .bar-1 {
    transform: translateY(8px) rotate(45deg);
  }
  .bar-2 {
    opacity: 0;
  }
  .bar-3 {
    transform: translateY(-8px) rotate(-45deg);
  }
  
  .menu {
    position: absolute;
    top: -250px;
    left: 0;
    width: 100%; /* Было 100%, меняем на фиксированную или процентную ширину */
    min-width: 400px; /* Новая минимальная ширина */
    background-color: #484065;
    color: white;
    padding: 30px; 
    display: flex;
    justify-content: space-around;
    align-items: center;
    transition: top 0.3s ease;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15); 
    margin-left: auto; 
    margin-right: auto; 
    right: 0; 
  }
  
  .menu.open {
    top: 0;
  }
  
  /* Увеличиваем элементы меню */
  .menu-item {
    display: flex;
    align-items: center;
    gap: 15px; /* Увеличиваем расстояние между иконкой и текстом */
    font-size: 16px; /* Увеличиваем размер текста */
    padding: 10px 15px; /* Добавляем отступы вокруг элементов */
  }
  
  .icon {
    width: 24px; /* Увеличиваем иконки */
    height: 24px;
  }
  
  /* Адаптация для мобильных */
  @media (max-width: 768px) {
    .menu {
      min-width: 100%; /* На мобильных - на всю ширину */
      flex-direction: column;
      padding: 60px 30px 30px; /* Увеличиваем отступы */
      gap: 20px; /* Больше расстояния между элементами */
    }
    
    .menu-item {
      font-size: 18px; /* Увеличиваем текст на мобильных */
    }
    
    .icon {
      width: 28px;
      height: 28px;
    }
  }
  </style>