<template>
    <div class="grid-table">       
        <!-- Строки данных -->
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
</template>

<script setup>

</script>

<style scoped>
    .grid-table {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-template-rows: auto auto auto auto auto auto 80px; /* Указываем высоты для каждой строки */
            gap: 1px;
            background-color: #ddd;
            border: 1px solid #ddd;
            margin: 0px;
            margin-top: 20px;
            max-width: 100%;
        }

        .header, .cell {
            background-color: white;
            padding: 10px;
            border: 1px solid #ccc;
        }

        .header {
            font-weight: bold;
            text-align: center;
            background-color: #f0f0f0;
        }

        /* Пример объединения ячеек */
        .cell-merged {
            background-color: white;
            grid-column: span 2; /* Объединение 2 колонок */
        }

        .cell-merged3{
            background-color: white;
            grid-column: span;
        }

        .row-merged {
            grid-row: span 2; /* Объединение 2 строк */
        }
        .row-3merged{
            grid-row: span 3;
        }
        @media (max-width: 768px) {
    .grid-row {
      grid-template-columns: repeat(5, minmax(80px, 1fr));
    }
    
    .cell {
      padding: 8px 4px;
      font-size: 14px;
    }
  }
  
  @media (max-width: 480px) {
    .grid-row {
      grid-template-columns: repeat(5, minmax(60px, 1fr));
    }
    
    .cell {
      padding: 6px 3px;
      font-size: 12px;
    }
  }
</style>

