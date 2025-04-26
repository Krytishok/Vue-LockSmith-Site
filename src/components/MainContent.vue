<template>
  <div class="main-content">
    <div class="content-wrapper">
      <h1>24/7 Emergency LockSmith Services</h1>
      <h2>Contact us now for prompt and Efficient LockSmith Services</h2>
    </div>
  </div>
</template>

<script setup>
</script>

<style scoped>
.main-content {
  display: flex;
  flex: 1;
  justify-content: left; /* Центрирование по горизонтали */
  align-items: center; /* Центрирование по вертикали */
  padding: 20px;
  color: white;
  text-align: center; /* Центрирование текста внутри элементов */
}

.content-wrapper {
  max-width: 800px; /* Ограничение ширины текстового блока */
  width: 100%;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
  line-height: 1.2;
}

h2 {
  font-size: 1.5rem;
  margin-bottom: 40px;
  color: white;
  font-weight: normal;
}

@media (max-width: 768px) {
  .main-content {
    padding: 20px;
    padding-top: 80px;
    text-shadow: 
    -0.5px -0.5px 0 black,
    0.5px -0.5px 0 black,
    -0.5px 0.5px 0 black,
    0.5px 0.5px 0 black;
  }
  
  h1 {
    font-size: 1.8rem;
  }
  
  h2 {
    font-size: 1.2rem;
  }
}
</style>