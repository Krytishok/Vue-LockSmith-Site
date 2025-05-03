<template>
    <div class="contact-form-wrapper">
      <div class="contact-form">
        <form @submit.prevent="submitForm" class="form-container">
          <div class="form-group">
            <input
              type="text"
              id="name"
              v-model="form.name"
              @input="validateName"
              :class="{ 'invalid': errors.name }"
              placeholder="Your Name"
              class="form-input"
            />
            <span class="error" v-if="errors.name">{{ errors.name }}</span>
          </div>
          
          <div class="form-group">
            <input
              type="tel"
              id="phone"
              v-model="form.phone"
              @input="validatePhone"
              :class="{ 'invalid': errors.phone }"
              placeholder="Phone Number"
              class="form-input"
            />
            <span class="error" v-if="errors.phone">{{ errors.phone }}</span>
          </div>
          
          <div class="form-group">
            <input
              type="email"
              id="email"
              v-model="form.email"
              @blur="validateEmail"
              :class="{ 'invalid': errors.email }"
              placeholder="Email Address"
              class="form-input"
            />
            <span class="error" v-if="errors.email">{{ errors.email }}</span>
          </div>
          
          <div class="form-group">
            <textarea
              id="message"
              v-model="form.message"
              @input="validateMessage"
              :class="{ 'invalid': errors.message }"
              placeholder="Your Message"
              class="form-textarea"
            ></textarea>
            <span class="error" v-if="errors.message">{{ errors.message }}</span>
          </div>
          
          <div class="button-container">
            <button type="submit" :disabled="!isFormValid" class="submit-btn">
              Contact Us
            </button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, reactive } from 'vue';
  
  const form = reactive({
    name: '',
    phone: '',
    email: '',
    message: ''
  });
  
  const errors = reactive({
    name: '',
    phone: '',
    email: '',
    message: ''
  });
  
  const validateName = () => {
    const nameRegex = /^[a-zA-Z\s'-]+$/;
    if (!form.name.trim()) {
      errors.name = '';
    } else if (!nameRegex.test(form.name)) {
      errors.name = 'Invalid characters in name';
    } else if (form.name.length < 2) {
      errors.name = 'Name is too short';
    } else {
      errors.name = '';
    }
  };
  
  const validatePhone = () => {
    const phoneRegex = /^[\d\s\-+()]{10,16}$/;
    if (!form.phone.trim()) {
      errors.phone = '';
    } else if (!phoneRegex.test(form.phone)) {
      errors.phone = 'Enter correct digit phone number';
    } else {
      errors.phone = '';
    }
  };
  
  const validateEmail = () => {
    const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
    if (!form.email.trim()) {
      errors.email = '';
    } else if (!emailRegex.test(form.email)) {
      errors.email = 'Please enter a valid email';
    } else {
      errors.email = '';
    }
  };
  
  const validateMessage = () => {
    if (!form.message.trim()) {
      errors.message = '';
    } else if (form.message.length < 10) {
      errors.message = 'Message should be at least 10 characters';
    } else if (form.message.length > 500) {
      errors.message = 'Message is too long (max 500 chars)';
    } else {
      errors.message = '';
    }
  };
  
  const isFormValid = computed(() => {
    return (
      form.name.trim().length >= 2 &&
      form.phone.trim().length >= 10 &&
      emailRegex.test(form.email) &&
      form.message.trim().length >= 10 &&
      !errors.name &&
      !errors.phone &&
      !errors.email &&
      !errors.message
    );
  });
  
  const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
  
  const submitForm = () => {
    validateName();
    validatePhone();
    validateEmail();
    validateMessage();
    
    if (isFormValid.value) {
      alert('Thank you! We will contact you soon.');
      console.log('Form data:', form);
      
      form.name = '';
      form.phone = '';
      form.email = '';
      form.message = '';
    }
  };
  </script>
  
  <style scoped>
  .contact-form-wrapper {
    flex: 1;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    padding-right: 10%;
    margin: 20px 0;
  }
  
  .contact-form {
    background-color: white;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
    width: 380px;
    padding: 35px 25px;
  }
  
  .form-title {
    color: #2c3e50;
    margin-bottom: 20px;
    font-size: 1.4rem;
    text-align: center;
    font-weight: 600;
  }
  
  .form-container {
    display: flex;
    flex-direction: column;
    gap: 15px;
  }
  
  .form-group {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }
  
  .form-input, .form-textarea {
    padding: 10px 12px;
    border: 1px solid #e0e3e7;
    border-radius: 8px;
    font-size: 14px;
    transition: all 0.3s ease;
    background-color: #f8fafc;
  }
  
  .form-input:focus, .form-textarea:focus {
    outline: none;
    border-color: #3b82f6;
    box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
  }
  
  .form-input::placeholder, .form-textarea::placeholder {
    color: #94a3b8;
  }
  
  .form-textarea {
    min-height: 90px;
    resize: vertical;
  }
  
  .invalid {
    border-color: #ef4444;
    background-color: #fef2f2;
  }
  
  .error {
    color: #ef4444;
    font-size: 0.7rem;
    height: 12px;
    margin-top: 2px;
    padding-left: 5px;
  }
  
  .button-container {
    margin-top: 10px;
    padding-top: 5px;
    border-top: 1px solid #f1f5f9;
  }
  
  .submit-btn {
    background-color: #3b82f6;
    color: white;
    padding: 12px;
    width: 100%;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 14px;
    font-weight: 500;
    transition: all 0.3s ease;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  
  .submit-btn:hover {
    background-color: #2563eb;
    transform: translateY(-1px);
  }
  
  .submit-btn:disabled {
    background-color: #94a3b8;
    cursor: not-allowed;
    transform: none;
    opacity: 0.7;
  }
  
  @media (max-width: 1024px) {
    .contact-form-wrapper {
      padding-right: 5%;
    }
  }
  
  @media (max-width: 768px) {
    .contact-form-wrapper {
      width: 100%;
      justify-content: center;
      padding-right: 0;
    }
    
    .contact-form {
      width: 90%;
      padding: 20px;
      margin: 15px 0;
    }
    
    .form-title {
      font-size: 1.3rem;
    }
  }
  </style>