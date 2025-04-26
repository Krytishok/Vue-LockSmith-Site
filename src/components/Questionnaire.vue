<template>
    <div class="form-container">
      <h2>Анкета заказа услуги вскрытия замков</h2>
      <form @submit.prevent="submitForm">
        <!-- Вопрос 1 -->
        <div class="form-group" :class="{ 'error': v$.q1.$error }">
          <label>1. Тип замка:</label>
          <select v-model="form.q1" @blur="v$.q1.$touch()">
            <option value="">Выберите тип</option>
            <option value="cylinder">Цилиндровый</option>
            <option value="lever">Сувальдный</option>
            <option value="electronic">Электронный</option>
            <option value="other">Другой</option>
          </select>
          <span class="error-message" v-if="v$.q1.$error">Это обязательный вопрос</span>
        </div>
  
        <!-- Вопрос 2 -->
        <div class="form-group" :class="{ 'error': v$.q2.$error }">
          <label>2. Местонахождение объекта:</label>
          <input 
            type="text" 
            v-model="form.q2" 
            @blur="v$.q2.$touch()"
            placeholder="Адрес или ориентир"
          >
          <span class="error-message" v-if="v$.q2.$error">Укажите местонахождение</span>
        </div>
  
        <!-- Вопрос 3 -->
        <div class="form-group">
          <label>3. Срочность:</label>
          <div class="radio-group">
            <label>
              <input type="radio" v-model="form.q3" value="immediately"> Немедленно (в течение часа)
            </label>
            <label>
              <input type="radio" v-model="form.q3" value="today"> Сегодня
            </label>
            <label>
              <input type="radio" v-model="form.q3" value="planned"> Запланированный визит
            </label>
          </div>
        </div>
  
        <!-- Вопрос 4 -->
        <div class="form-group" :class="{ 'error': v$.q4.$error }">
          <label>4. Контактный телефон:</label>
          <input 
            type="tel" 
            v-model="form.q4" 
            @blur="v$.q4.$touch()"
            placeholder="+7 (XXX) XXX-XX-XX"
          >
          <span class="error-message" v-if="v$.q4.$error">Введите корректный телефон</span>
        </div>
  
        <!-- Вопрос 5 -->
        <div class="form-group">
          <label>5. Нужна ли замена замка?</label>
          <div class="radio-group">
            <label>
              <input type="radio" v-model="form.q5" value="yes"> Да
            </label>
            <label>
              <input type="radio" v-model="form.q5" value="no"> Нет
            </label>
            <label>
              <input type="radio" v-model="form.q5" value="dontknow"> Не знаю
            </label>
          </div>
        </div>
  
        <!-- Вопрос 6 -->
        <div class="form-group">
          <label>6. Как произошло закрытие?</label>
          <textarea v-model="form.q6" placeholder="Опишите ситуацию"></textarea>
        </div>
  
        <!-- Вопрос 7 -->
        <div class="form-group">
          <label>7. Доступны ли ключи внутри помещения?</label>
          <select v-model="form.q7">
            <option value="">Выберите вариант</option>
            <option value="yes">Да, есть внутри</option>
            <option value="no">Нет, утеряны</option>
            <option value="broken">Сломаны в замке</option>
          </select>
        </div>
  
        <!-- Вопрос 8 -->
        <div class="form-group">
          <label>8. Дополнительные пожелания:</label>
          <textarea v-model="form.q8" placeholder="Особые требования"></textarea>
        </div>
  
        <!-- Вопрос 9 -->
        <div class="form-group">
          <label>9. Как с вами удобнее связаться?</label>
          <div class="checkbox-group">
            <label>
              <input type="checkbox" v-model="form.q9" value="call"> Телефонный звонок
            </label>
            <label>
              <input type="checkbox" v-model="form.q9" value="sms"> SMS
            </label>
            <label>
              <input type="checkbox" v-model="form.q9" value="whatsapp"> WhatsApp
            </label>
          </div>
        </div>
  
        <!-- Вопрос 10 -->
        <div class="form-group">
          <label>10. Когда вам удобно принять мастера?</label>
          <input type="datetime-local" v-model="form.q10">
        </div>
  
        <button type="submit" :disabled="v$.$invalid">Отправить заявку</button>
        <div v-if="submitSuccess" class="success-message">
          Спасибо! Ваша заявка отправлена. Мы свяжемся с вами в ближайшее время.
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import { reactive, computed } from 'vue'
  import useVuelidate from '@vuelidate/core'
  import { required, helpers } from '@vuelidate/validators'
  
  const phoneRegex = /^(\+7|8)[\s-]?\(?\d{3}\)?[\s-]?\d{3}[\s-]?\d{2}[\s-]?\d{2}$/
  
  export default {
    setup() {
      const form = reactive({
        q1: '',
        q2: '',
        q3: 'immediately',
        q4: '',
        q5: 'no',
        q6: '',
        q7: '',
        q8: '',
        q9: [],
        q10: ''
      })
  
      const rules = computed(() => ({
        q1: { required },
        q2: { required },
        q4: { 
          required,
          phone: helpers.withMessage('Некорректный номер', value => phoneRegex.test(value))
        }
      }))
  
      const v$ = useVuelidate(rules, form)
  
      return { form, v$ }
    },
    data() {
      return {
        submitSuccess: false
      }
    },
    methods: {
      async submitForm() {
        const isFormValid = await this.v$.$validate()
        if (!isFormValid) return
        
        // Здесь обычно отправка на сервер
        console.log('Форма отправлена:', this.form)
        this.submitSuccess = true
        
        // Сброс формы через 5 секунд
        setTimeout(() => {
          this.resetForm()
        }, 5000)
      },
      resetForm() {
        this.form = {
          q1: '',
          q2: '',
          q3: 'immediately',
          q4: '',
          q5: 'no',
          q6: '',
          q7: '',
          q8: '',
          q9: [],
          q10: ''
        }
        this.v$.$reset()
        this.submitSuccess = false
      }
    }
  }
  </script>
  
  <style scoped>
  .form-container {
    background-color: whitesmoke;
    border-radius: 20px;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  
  h2 {
    color: #2c3e50;
    text-align: center;
    margin-bottom: 30px;
  }
  
  .form-group {
    margin-bottom: 20px;
    width: 97%;
  }
  
  label {
    display: block;
    margin-bottom: 8px;
    font-weight: bold;
  }
  
  input[type="text"],
  input[type="tel"],
  input[type="datetime-local"],
  select,
  textarea {
    width: 100%;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 16px;
  }

  
  textarea {
    min-height: 100px;
    resize: vertical;
  }
  
  .radio-group,
  .checkbox-group {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  
  .radio-group label,
  .checkbox-group label {
    display: flex;
    align-items: center;
    font-weight: normal;
    cursor: pointer;
  }
  
  .radio-group input,
  .checkbox-group input {
    margin-right: 10px;
  }
  
  button {
    background-color: #42b983;
    color: white;
    border: none;
    padding: 12px 20px;
    font-size: 16px;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  button:hover {
    background-color: #33a06f;
  }
  
  button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
  }
  
  .error-message {
    color: #e74c3c;
    font-size: 14px;
    margin-top: 5px;
    display: block;
  }
  
  .error input,
  .error select {
    border-color: #e74c3c;
  }
  
  .success-message {
    margin-top: 20px;
    padding: 15px;
    background-color: #d4edda;
    color: #155724;
    border-radius: 4px;
  }
  </style>