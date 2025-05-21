<template>
  <div class="contact-form-wrapper">
    <div class="contact-form">
      <form @submit.prevent="submitForm" class="form-container">
        <div class="form-group">
          <input
            type="text"
            id="name"
            v-model="form.name"
            @input="handleNameInput"
            @focus="hideError('name')"
            @blur="validateName"
            :class="{ 'invalid': errors.name }"
            placeholder="Your Name"
            class="form-input"
          />
          <div class="error-notification" v-if="showErrors.name && errors.name">
            <span class="error-icon">!</span>
            <span class="error-text">{{ errors.name }}</span>
          </div>
        </div>
        
        <div class="form-group">
          <input
            type="tel"
            id="phone"
            v-model="form.phone"
            @input="formatRussianPhone"
            @focus="hideError('phone')"
            @blur="validatePhone"
            :class="{ 'invalid': errors.phone }"
            placeholder="+7 (XXX) XXX-XX-XX"
            class="form-input"
          />
          <div class="error-notification" v-if="showErrors.phone && errors.phone">
            <span class="error-icon">!</span>
            <span class="error-text">{{ errors.phone }}</span>
          </div>
        </div>
        
        <div class="form-group">
          <input
            type="email"
            id="email"
            v-model="form.email"
            @input="handleEmailInput"
            @focus="hideError('email')"
            @blur="validateEmail"
            :class="{ 'invalid': errors.email }"
            placeholder="Email Address"
            class="form-input"
          />
          <div class="error-notification" v-if="showErrors.email && errors.email">
            <span class="error-icon">!</span>
            <span class="error-text">{{ errors.email }}</span>
          </div>
        </div>
        
        <div class="form-group">
          <textarea
            id="message"
            v-model="form.message"
            @focus="hideError('message')"
            @blur="validateMessage"
            :class="{ 'invalid': errors.message }"
            placeholder="Your Message"
            class="form-textarea"
          ></textarea>
          <div class="error-notification" v-if="showErrors.message && errors.message">
            <span class="error-icon">!</span>
            <span class="error-text">{{ errors.message }}</span>
          </div>
        </div>
        
        <div class="button-container">
          <button type="submit" :disabled="!isFormValid" class="submit-btn">
            CONTACT US
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

const showErrors = reactive({
  name: false,
  phone: false,
  email: false,
  message: false
});

const hideError = (field) => {
  showErrors[field] = false;
};

const handleNameInput = (e) => {
  form.name = e.target.value.replace(/\s+/g, ' ');
  validateName();
};

const handleEmailInput = (e) => {
  form.email = e.target.value.replace(/\s+/g, '');
  validateEmail();
};

const formatRussianPhone = (e) => {
  let input = e.target.value.replace(/\D/g, '');
  if (input.length > 11) input = input.substring(0, 11);
  
  let formatted = '';
  if (input.length > 0) {
    formatted = '+7';
    if (input.length > 1) {
      formatted += ' (' + input.substring(1, 4);
    }
    if (input.length > 4) {
      formatted += ') ' + input.substring(4, 7);
    }
    if (input.length > 7) {
      formatted += '-' + input.substring(7, 9);
    }
    if (input.length > 9) {
      formatted += '-' + input.substring(9, 11);
    }
  }
  
  form.phone = formatted;
  validatePhone();
};

const validateName = () => {
  const nameRegex = /^[a-zA-Z\s'-]+$/;
  showErrors.name = true;
  
  if (!form.name.trim()) {
    errors.name = 'Name is required';
  } else if (form.name.length > 50){
    errors.name = "Name is too long"
  } else if (!nameRegex.test(form.name)) {
    errors.name = 'Only letters and hyphens allowed';
  } else if (form.name.length < 2) {
    errors.name = 'Name is too short';
  } else {
    errors.name = '';
  }
};

const validatePhone = () => {
  const digitsOnly = form.phone.replace(/\D/g, '');
  showErrors.phone = true;
  
  if (!digitsOnly) {
    errors.phone = 'Phone number is required';
  } else if (digitsOnly.length !== 11 || !digitsOnly.startsWith('7')) {
    errors.phone = 'Enter valid Russian number (+7 XXX XXX-XX-XX)';
  } else {
    errors.phone = '';
  }
};

const validateEmail = () => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/;
  const popularDomains = ['com', 'ru', 'net', 'org', 'io', 'gov', 'edu'];
  const disposableDomains = ['tempmail.com', 'mailinator.com'];
  
  showErrors.email = true;
  const email = form.email.trim();
  
  if (!email) {
    errors.email = 'Email is required';
  } else if (/\s/.test(email)) {
    errors.email = 'Email cannot contain spaces';
  } else if (!emailRegex.test(email)) {
    errors.email = 'Please enter a valid email';
  } else if (disposableDomains.some(domain => email.includes(domain))) {
    errors.email = 'Temporary emails are not allowed';
  } else {
    const domain = email.split('.').pop().toLowerCase();
    if (!popularDomains.includes(domain)) {
      errors.email = 'Please use a common domain (.com, .ru, etc.)';
    } else {
      errors.email = ''; // Валидация пройдена
    }
  }
};

const validateMessage = () => {
  showErrors.message = true;
  
  if (!form.message.trim()) {
    errors.message = 'Message is required';
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
    form.phone.replace(/\D/g, '').length === 11 &&
    /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email) &&
    !/\s/.test(form.email) &&
    form.message.trim().length >= 10 &&
    !errors.name &&
    !errors.phone &&
    !errors.email &&
    !errors.message
  );
});

const submitForm = () => {
  validateName();
  validatePhone();
  validateEmail();
  validateMessage();
  
  if (isFormValid.value) {
    alert('Thank you! We will contact you soon.');
    console.log('Form data:', {
      name: form.name,
      phone: form.phone.replace(/\D/g, ''),
      email: form.email,
      message: form.message
    });
    
    form.name = '';
    form.phone = '';
    form.email = '';
    form.message = '';
    Object.keys(showErrors).forEach(key => showErrors[key] = false);
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
  padding: 25px;
  position: relative;
  box-sizing: border-box;
}

.form-container {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.form-group {
  position: relative;
  margin-bottom: 8px;
  padding-top: 6px;
}

.form-input, .form-textarea {
  padding: 10px 12px;
  border: 1px solid #e0e3e7;
  border-radius: 8px;
  font-size: 14px;
  transition: all 0.3s ease;
  background-color: #f8fafc;
  width: 100%;
  box-sizing: border-box;
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

.error-notification {
  position: absolute;
  top: 0;
  left: 12px;
  display: flex;
  align-items: center;
  gap: 4px;
  color: #ef4444;
  font-size: 0.7rem;
  padding: 2px 6px;
  background-color: #fff;
  border-radius: 4px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  z-index: 1;
  border: 1px solid #ef4444;
  transform: translateY(-50%);
  animation: fadeIn 0.2s ease;
  white-space: nowrap;
  line-height: 1.2;
}

.error-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 14px;
  height: 14px;
  background-color: #ef4444;
  color: white;
  border-radius: 50%;
  font-size: 0.6rem;
  font-weight: bold;
  flex-shrink: 0;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-30%); }
  to { opacity: 1; transform: translateY(-50%); }
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
  letter-spacing: 0.5px;
}

.submit-btn:not(:disabled) {
  background-color: #1d4ed8;
  box-shadow: 0 2px 4px rgba(29, 78, 216, 0.3);
}

.submit-btn:hover:not(:disabled) {
  background-color: #1e40af;
  transform: translateY(-1px);
  box-shadow: 0 4px 6px rgba(30, 64, 175, 0.3);
}

.submit-btn:disabled {
  background-color: #3b82f6;
  opacity: 0.7;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
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
}
</style>