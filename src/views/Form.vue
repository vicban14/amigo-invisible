<template>
  <div class="form">
    <form @submit.prevent="sendEmail">
      <div v-for="(field, index) in fields" :key="index">
        <label>
          Name:
          <input v-model="field.name" type="text" required />
        </label>
        <label>
          Email:
          <input v-model="field.email" type="email" required />
        </label>
      </div>
      <button type="button" @click="addFields">Add Another Field</button>
      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from 'vue';
import emailjs from 'emailjs-com';
import { emailConfig } from '../emailConfig';

interface Field {
  name: string;
  email: string;
}

export default defineComponent({
  setup() {
    const fields = reactive<Field[]>([{ name: '', email: '' }]);

    const addFields = () => {
      fields.push({ name: '', email: '' });
    };

    const sendEmail = () => {
      fields.forEach((field) => {
        const templateParams = {
          name: field.name,
          email: field.email,
        };
        console.log('Service ID:', import.meta.env.VITE_EMAIL_SERVICE_ID);
        console.log('Template ID:', import.meta.env.VITE_EMAIL_TEMPLATE_ID);
        console.log('User ID:', import.meta.env.VITE_EMAIL_USER_ID);

        emailjs
          .send(emailConfig.serviceId, emailConfig.templateId, templateParams, emailConfig.userId)
          .then(
            (response) => {
              console.log('Email sent successfully!', response.status, response.text);
            },
            (error) => {
              console.error('Failed to send email.', error);
            }
          );
      });
    };

    return {
      fields,
      addFields,
      sendEmail,
    };
  },
});
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

button {
  padding: 0.5rem 1rem;
  margin-top: 1rem;
}
</style>
