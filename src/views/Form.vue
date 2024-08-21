<template>
  <div class="form-page">
    <div class="background-image"></div>
    
    <div class="form-container">
      <h1>Selecciona los participantes</h1>
      <p class="instructions">Pulsa "Añadir otro amigo" tantas veces como participantes quieres que haya. Completa el formulario y selecciona "Enviar" para enviar los emails.</p>
      
      <form @submit.prevent="sendEmail">
        <div v-for="(field, index) in fields" :key="index" class="form-field">
          <span>Participante {{index + 1}}</span>
          <label>
            <input v-model="field.name" type="text" placeholder="Nombre" required />
          </label>
          <label>
            <input v-model="field.email" type="email" placeholder="Email" required />
          </label>
        </div>
        <button type="button" class="add-button" @click="addFields">Añadir otro amigo</button>
        <button type="submit" class="submit-button">Enviar</button>
      </form>
    </div>
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
        newArray = shuffleArray([...originalArray]);
        attempts++;
      } while (hasCoincidences(originalArray, newArray) && attempts < maxAttempts);

      if (attempts >= maxAttempts) {
        throw new Error('Unable to generate a unique array after maximum attempts');
      }

      return newArray;
    }

    const sendEmail = () => {
      const beneficiaryNamesList = generateUniqueArray(fields.map(field => field.name));

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
      sendEmail
    };
  }
});
</script>

<style scoped>
.form-page {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
}

.background-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-image: url('/secret-santa-wallpaper.jpg');
  background-size: cover;
  background-position: center;
  z-index: -1;
}

.form-container {
  background-color: rgba(255, 255, 255, 0.9);
  padding: 2rem;
  border-radius: 20px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  max-width: 70vw;
  text-align: center;
  max-height: 90vh;
  overflow-y: auto;
}

h1 {
  font-family: 'Source Sans Pro', sans-serif;
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 1rem;
}

.instructions {
  font-size: 1.2rem;
  margin-bottom: 2rem;
  color: #34495e;
}

.form-field {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

input {
  padding: 0.5rem;
  border-radius: 8px;
  border: 2px solid #c0392b;
  font-size: 1rem;
}

input:focus {
  outline: none;
  border-color: #e74c3c;
}

.add-button, .submit-button {
  padding: 0.8rem 1.5rem;
  font-size: 1.2rem;
  cursor: pointer;
  color: #fff;
  background-color: #e74c3c;
  border: 2px solid #fff;
  border-radius: 10px;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  margin-top: 1rem;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

.add-button:hover, .submit-button:hover {
  background-color: #c0392b;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.4);
}

.add-button:before, .submit-button:before {
  content: '';
  position: absolute;
  top: -5px;
  right: -5px;
  bottom: -5px;
  left: -5px;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.3);
  z-index: -1;
  transition: opacity 0.3s ease;
  opacity: 0;
}

.add-button:hover:before, .submit-button:hover:before {
  opacity: 1;
}

.add-button {
  background-color: #27ae60;
}

.add-button:hover {
  background-color: #2ecc71;
}
</style>
