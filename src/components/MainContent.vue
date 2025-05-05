<template>
  <div class="main-content">
    <div class="content-wrapper">
      <h1>
        <span class="first-line">24/7 Emergency</span>
        <span class="second-line">LockSmith Services</span>
      </h1>
      <div class="divider"></div>
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
  justify-content: left;
  align-items: center;
  padding: 20px;
  color: white;
  text-align: left;
}

.content-wrapper {
  max-width: 800px;
  width: 100%;
}

h1 {
  font-size: 3.5rem;
  margin-bottom: 15px;
  line-height: 1.2;
  font-weight: 700;
  text-shadow: 
    -1px -1px 0 #000,
    1px -1px 0 #000,
    -1px 1px 0 #000,
    1px 1px 0 #000;
}

h1 span {
  display: block;
}

h1 .first-line {
  font-size: 1.2em; /* Можно настроить отдельный размер */
}

h1 .second-line {
  font-size: 1em;
}

.divider {
  width: 80px;
  height: 4px;
  background-color: #00a0e9;
  margin: 20px 0;
}

h2 {
  font-size: 1.4rem;
  margin-top: 20px;
  color: white;
  font-weight: 300;
  line-height: 1.4;
}

/* Капитализация только первых букв */
h1, h2 {
  text-transform: capitalize;
}

@media (max-width: 768px) {
  .main-content {
    padding: 20px;
    padding-top: 80px;
    text-align: center;
  }
  
  h1 {
    font-size: 2rem;
  }

  h1 .first-line {
    font-size: 1.2em;
  }

  h1 .second-line {
    font-size: 1em;
  }
  
  h2 {
    font-size: 1.3rem;
  }

  .divider {
    width: 60px;
    height: 3px;
    margin: 15px auto;
  }
}

@media (max-width: 480px) {
  h1 {
    font-size: 1.8rem;
  }
  
  h2 {
    font-size: 1.1rem;
  }
}
</style>