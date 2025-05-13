<template>
  <div class="table-container" v-if="showTable">
    <div class="table-window">
      <div class="table-header">
        <h3>Table</h3>
        <button @click="closeTable" class="close-btn">×</button>
      </div>
      
      <div class="table-content">
        <div class="grid-table">       
          <div class="cell"> 1-1</div>
        <div class="cell row-merged">Объединены 1-2 2-2</div>
        <div class="cell row-merged">Объединены 1-3 2-3</div>
        <div class="cell row-merged">Объединены 1-4 2-4</div>
        <div class="cell">1-5</div>
        
        <div class="cell"> 2-1</div>
        <div class="cell"> 2-5</div>
        
        <div class="cell"> 3-1</div>
        <div class="cell row-3merged">Объединены 3-2 4-2 5-2</div>
        <div class="cell"> 3-3</div>
        <div class="cell"> 3-4</div>
        <div class="cell"> 3-5</div>
        
        <div class="cell row-merged"> 4-1 и 5-1</div>
        <div class="cell"> 4-3</div>
        <div class="cell"> 4-4</div>
        <div class="cell"> 4-5</div>
        


        <div class="cell row-3merged">Объединены 5-3 6-3 7-3</div>
        <div class="cell"> 5-4</div>
        <div class="cell"> 5-5</div>
        
        <div class="cell"> 6-1</div>
        <div class="cell"> 6-2</div>
        <div class="cell"> 6-4</div>
        <div class="cell"> 6-5</div>
        
        <div class="cell"> 7-1</div>
        <div class="cell"> 7-2</div>
        <div class="cell"> 7-4</div>
        <div class="cell"> 7-5</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const showTable = ref(false);

const closeTable = () => {
  showTable.value = false;
};

defineExpose({
  showTable
});
</script>

<style scoped>
.table-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.table-window {
  background-color: white;
  border-radius: 10px;
  width: 90%;
  max-width: 800px;
  max-height: 90vh;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.table-header {
  background-color: #3b82f6;
  color: white;
  padding: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-shrink: 0;
}

.table-header h3 {
  margin: 0;
  font-size: 1.2rem;
}

.close-btn {
  background: none;
  border: none;
  color: white;
  font-size: 1.5rem;
  cursor: pointer;
  padding: 0 10px;
}

.table-content {
  padding: 20px;
  overflow-y: auto;
}

.grid-table {
  display: grid;
  grid-template-columns: repeat(5, minmax(120px, 1fr));
  gap: 1px;
  background-color: #ddd;
  border: 1px solid #ddd;
  margin: 0;
}

.header, .cell {
  background-color: white;
  padding: 12px;
  border: 1px solid #ccc;
  min-height: 50px;
  display: flex;
  align-items: center;
}

.header {
  font-weight: bold;
  text-align: center;
  background-color: #f0f0f0;
  position: sticky;
  top: 0;
}

.row-merged {
  grid-row: span 2;
}

.row-3merged {
  grid-row: span 3;
}

@media (max-width: 768px) {
  .table-window {
    width: 95%;
    max-width: 95%;
  }
  
  .grid-table {
    grid-template-columns: repeat(5, minmax(80px, 1fr));
  }
  
  .cell {
    padding: 8px 4px;
    font-size: 14px;
  }
}

@media (max-width: 480px) {
  .grid-table {
    grid-template-columns: repeat(5, minmax(60px, 1fr));
    font-size: 12px;
  }
  
  .cell {
    padding: 6px 3px;
  }
  
  .table-header h3 {
    font-size: 1rem;
  }
}
</style>