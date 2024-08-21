<template>
  <div class="home">
    <div class="background-image"></div>

    <div class="title-container">
      <h1>Secret Santa</h1>
    </div>

    <div class="overlay">
      <p>Pulsa el botón de comenzar para crear tu amigo invisible!</p>
      <button @click="goToForm" class="christmas-button">Comenzar</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { useRouter } from 'vue-router';

export default defineComponent({
  setup() {
    const router = useRouter();

    const goToForm = () => {
      router.push('/form');
    };

    return {
      goToForm
    };
  }
});
</script>

<style scoped>
.home {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

.background-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-image: url('/3083158.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  overflow: hidden;
  z-index: -1;
}

.title-container {
  position: absolute;
  top: 10%;
  background-color: #e74c3c;
  padding: 1rem 2rem;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  z-index: 1;
  text-align: center;
}

.title-container h1 {
  font-size: 3rem;
  font-family: 'Source Sans Pro', sans-serif;
  color: #ffffff;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  margin: 0;
}

.overlay {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 60vw;
  padding: 2rem;
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 20px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  z-index: 1;
  text-align: center;
}

.overlay p {
  margin-bottom: 1rem;
  font-size: 1.5rem;
  color: #333;
}

.christmas-button {
  padding: 1rem 2rem;
  font-size: 1.5rem;
  cursor: pointer;
  background-color: #e74c3c;
  color: white;
  border: 2px solid #fff;
  border-radius: 10px;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  position: relative;
}

.christmas-button:before {
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

.christmas-button:hover {
  background-color: #c0392b;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.4);
}

.christmas-button:hover:before {
  opacity: 1;
}
</style>
