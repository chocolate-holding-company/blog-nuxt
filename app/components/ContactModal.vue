<template>
 <div class="modal-overlay" @click.self="$emit('close')">
  <div class="modal-content">
   <button
    class="close-button"
    @click="$emit('close')"
    aria-label="Close modal"
   >
    <svg
     width="24"
     height="24"
     viewBox="0 0 24 24"
     fill="none"
     stroke="currentColor"
     stroke-width="2"
    >
     <line x1="18" y1="6" x2="6" y2="18"></line>
     <line x1="6" y1="6" x2="18" y2="18"></line>
    </svg>
   </button>

   <div v-if="!submitted">
    <h2>Contact Us</h2>
    <p class="subtitle">
     Have a question or want to get in touch? Fill out the form below.
    </p>

    <form @submit.prevent="handleSubmit" class="contact-form">
     <div class="form-group">
      <label for="name">Name <span class="required">*</span></label>
      <input
       id="name"
       v-model="formData.name"
       type="text"
       :class="{ error: errors.name }"
       @input="clearError('name')"
      />
      <span v-if="errors.name" class="error-message">{{ errors.name }}</span>
     </div>

     <div class="form-group">
      <label for="email">Email <span class="required">*</span></label>
      <input
       id="email"
       v-model="formData.email"
       type="email"
       :class="{ error: errors.email }"
       @input="clearError('email')"
      />
      <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
     </div>

     <div class="form-group">
      <label for="message">Message <span class="required">*</span></label>
      <textarea
       id="message"
       v-model="formData.message"
       rows="5"
       :class="{ error: errors.message }"
       @input="clearError('message')"
      ></textarea>
      <span v-if="errors.message" class="error-message">{{
       errors.message
      }}</span>
     </div>

     <button type="submit" class="submit-button" :disabled="isSubmitting">
      {{ isSubmitting ? "Sending..." : "Send Message" }}
     </button>
    </form>
   </div>

   <div v-else class="success-message">
    <div class="success-icon">
     <svg
      width="64"
      height="64"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="2"
     >
      <circle cx="12" cy="12" r="10"></circle>
      <path d="M9 12l2 2 4-4"></path>
     </svg>
    </div>
    <h2>Thank You!</h2>
    <p>
     Your message has been sent successfully. We'll get back to you as soon as
     possible.
    </p>
    <button @click="$emit('close')" class="close-success-button">Close</button>
   </div>
  </div>
 </div>
</template>

<script setup>
import { ref, reactive } from "vue";

defineEmits(["close"]);

const formData = reactive({
 name: "",
 email: "",
 message: "",
});

const errors = reactive({
 name: "",
 email: "",
 message: "",
});

const submitted = ref(false);
const isSubmitting = ref(false);

const validateEmail = (email) => {
 const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
 return re.test(email);
};

const clearError = (field) => {
 errors[field] = "";
};

const validateForm = () => {
 let isValid = true;

 errors.name = "";
 errors.email = "";
 errors.message = "";

 if (!formData.name.trim()) {
  errors.name = "Name is required";
  isValid = false;
 } else if (formData.name.trim().length < 2) {
  errors.name = "Name must be at least 2 characters";
  isValid = false;
 }

 if (!formData.email.trim()) {
  errors.email = "Email is required";
  isValid = false;
 } else if (!validateEmail(formData.email)) {
  errors.email = "Please enter a valid email address";
  isValid = false;
 }

 if (!formData.message.trim()) {
  errors.message = "Message is required";
  isValid = false;
 } else if (formData.message.trim().length < 10) {
  errors.message = "Message must be at least 10 characters";
  isValid = false;
 }

 return isValid;
};

const handleSubmit = () => {
 if (validateForm()) {
  isSubmitting.value = true;

  setTimeout(() => {
   isSubmitting.value = false;
   submitted.value = true;

   formData.name = "";
   formData.email = "";
   formData.message = "";
  }, 1000);
 }
};
</script>

<style lang="scss" scoped>
@use "sass:color";
.modal-overlay {
 position: fixed;
 top: 0;
 left: 0;
 right: 0;
 bottom: 0;
 background-color: rgba(0, 0, 0, 0.7);
 display: flex;
 align-items: center;
 justify-content: center;
 z-index: 1000;
 padding: $spacing-md;
 animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
 from {
  opacity: 0;
 }
 to {
  opacity: 1;
 }
}

.modal-content {
 background-color: white;
 border-radius: $border-radius;
 padding: $spacing-lg;
 max-width: 600px;
 width: 100%;
 max-height: 90vh;
 overflow-y: auto;
 position: relative;
 animation: slideUp 0.3s ease;

 @media (max-width: $breakpoint-mobile) {
  padding: $spacing-md;
 }
}

@keyframes slideUp {
 from {
  transform: translateY(20px);
  opacity: 0;
 }
 to {
  transform: translateY(0);
  opacity: 1;
 }
}

.close-button {
 position: absolute;
 top: $spacing-md;
 right: $spacing-md;
 background: none;
 border: none;
 color: $text-dark;
 cursor: pointer;
 padding: $spacing-xs;
 display: flex;
 align-items: center;
 justify-content: center;
 border-radius: 50%;
 transition: $transition;

 &:hover {
  background-color: $secondary-color;
 }
}

h2 {
 color: $primary-color;
 margin-bottom: $spacing-sm;
}

.subtitle {
 color: #666;
 margin-bottom: $spacing-lg;
}

.contact-form {
 .form-group {
  margin-bottom: $spacing-md;

  label {
   display: block;
   margin-bottom: $spacing-xs;
   font-weight: 600;
   color: $text-dark;

   .required {
    color: $primary-color;
   }
  }

  input,
  textarea {
   width: 100%;
   padding: $spacing-sm;
   border: 2px solid $border-color;
   border-radius: $border-radius;
   font-family: inherit;
   font-size: 1rem;
   transition: $transition;

   &:focus {
    outline: none;
    border-color: $primary-color;
   }

   &.error {
    border-color: $primary-color;
    background-color: rgba(196, 30, 58, 0.05);
   }
  }

  textarea {
   resize: vertical;
   min-height: 120px;
  }

  .error-message {
   display: block;
   color: $primary-color;
   font-size: 0.875rem;
   margin-top: $spacing-xs;
  }
 }

 .submit-button {
  width: 100%;
  padding: $spacing-sm $spacing-lg;
  background-color: $primary-color;
  color: $text-light;
  border: none;
  border-radius: $border-radius;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: $transition;

  &:hover:not(:disabled) {
   background: color.adjust(#c41e3a, $lightness: -10%);
   transform: translateY(-2px);
   box-shadow: 0 4px 12px $shadow;
  }

  &:disabled {
   opacity: 0.6;
   cursor: not-allowed;
  }
 }
}

.success-message {
 text-align: center;
 padding: $spacing-lg 0;

 .success-icon {
  color: $primary-color;
  margin-bottom: $spacing-md;
  animation: scaleIn 0.5s ease;

  svg {
   margin: 0 auto;
  }
 }

 @keyframes scaleIn {
  from {
   transform: scale(0);
  }
  to {
   transform: scale(1);
  }
 }

 h2 {
  color: $primary-color;
  margin-bottom: $spacing-md;
 }

 p {
  color: #666;
  margin-bottom: $spacing-lg;
 }

 .close-success-button {
  padding: $spacing-sm $spacing-lg;
  background-color: $primary-color;
  color: $text-light;
  border: none;
  border-radius: $border-radius;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: $transition;

  &:hover {
   background: color.adjust(#c41e3a, $lightness: -10%);
   transform: translateY(-2px);
   box-shadow: 0 4px 12px $shadow;
  }
 }
}
</style>
