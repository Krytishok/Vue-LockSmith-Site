<template>
    <div class="corner-icons">
      <a href="https://docs.google.com/spreadsheets/d/1aiH4Ino-VoNpErZQHqqEJEcx38kbBZP5NJAy2jFu5yQ/edit?gid=0#gid=0" class="icon-link">
        <svg class="icon" viewBox="0 0 24 24">
          <path d="M12 4l-8 8h5v8h6v-8h5z"/>
        </svg>
      </a>
      <a href="mailto:info@example.com" class="icon-link">
        <svg class="icon" viewBox="0 0 24 24">
          <path d="M22 6c0-1.1-.9-2-2-2H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6zm-2 0l-8 5-8-5h16zm0 12H4V8l8 5 8-5v10z"/>
        </svg>
      </a>
      <a href="tel:+1234567890" class="icon-link">
        <svg class="icon" viewBox="0 0 24 24">
          <path d="M20 15.5c-1.2 0-2.5-.2-3.6-.6h-.3c-.3 0-.5.1-.7.3l-2.2 2.2c-2.8-1.5-5.2-3.8-6.6-6.6l2.2-2.2c.3-.3.4-.7.2-1-.3-1.1-.5-2.4-.5-3.6 0-.5-.5-1-1-1H4c-.5 0-1 .5-1 1 0 9.4 7.6 17 17 17 .5 0 1-.5 1-1v-3.5c0-.5-.5-1-1-1z"/>
        </svg>
      </a>
    </div>
  </template>
  
  <script setup>
  </script>
  
  <style scoped>
  .corner-icons {
    position: fixed;
    top: 20px;
    right: 20px;
    display: flex;
    gap: 15px;
    z-index: 1000;
  }
  
  .icon-link {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    background-color: none;
    border-radius: 50%;
    box-shadow: none;
    transition: all 0.3s ease;
  }
  
  .icon-link:hover {
    background-color: white;
    transform: translateY(-2px);
  }
  
  .icon-link:hover .icon {
    fill: white;
  }
  
  .icon {
    width: 20px;
    height: 20px;
    fill: white;
    transition: fill 0.3s ease;
  }
  
  @media (max-width: 768px) {
    .corner-icons {
      top: 10px;
      right: 10px;
      gap: 10px;
      visibility: hidden;
    }
    
    .icon-link {
      width: 36px;
      height: 36px;
    }
    
    .icon {
      width: 18px;
      height: 18px;
    }
  }
  </style>