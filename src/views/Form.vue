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

    function shuffleArray<T>(array: T[]): T[] {
      let currentIndex = array.length;
      let randomIndex: number;

      while (currentIndex !== 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
      }

      return array;
    }

    function hasCoincidences(arr1: any[], arr2: any[]): boolean {
      return arr1.some((value, index) => value === arr2[index]);
    }

    function generateUniqueArray<T>(originalArray: T[], maxAttempts: number = 10): T[] {
      let attempts = 0;
      let newArray = [...originalArray];

      do {
        newArray = shuffleArray([...originalArray]); // Generar una copia aleatoria
        attempts++;
      } while (hasCoincidences(originalArray, newArray) && attempts < maxAttempts);

      if (attempts >= maxAttempts) {
        throw new Error('Unable to generate a unique array after maximum attempts');
      }

      return newArray;
    }


    const sendEmail = () => {
      const beneficiaryNamesList = generateUniqueArray(fields.map(field => field.name))

      fields.forEach((field, index) => {
        const templateParams = {
          userName: field.name,
          email: field.email,
          beneficiary: beneficiaryNamesList[index]
        };

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
