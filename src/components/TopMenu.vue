<template>
    <div class="menu">
      <div class="menu-item">📧 Email: locksmith@example.com</div>
      <div class="menu-item">📞 Phone: +1 (800) 123-4567</div>
      <div class="menu-item">➡️ Navigate to: 123 Main St</div>
    </div>
  </template>
  
  <script setup>
  </script>
  
  <style scoped>
  .menu {
    position: absolute;
    top: 3rem;
    left: 0;
    width: 100%;
    background-color: #222;
    color: white;
    padding: 1rem;
    animation: slideDown 0.3s ease-out;
  }
  .menu-item {
    margin: 0.5rem 0;
  }
  @keyframes slideDown {
    from { transform: translateY(-100%); }
    to { transform: translateY(0); }
  }
  </style>