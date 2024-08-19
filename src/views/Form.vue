<template>
  <div class="form">
    <h1>Form</h1>
    <form @submit.prevent="submitForm">
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

    const submitForm = () => {
      const emails = fields.map(field => field.email);

      // Here you would make the request to the backend to send the emails
      console.log("Sending emails to:", emails);

      alert('Form submitted');
    };

    return {
      fields,
      addFields,
      submitForm
    };
  }
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
