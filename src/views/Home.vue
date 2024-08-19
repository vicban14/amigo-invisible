<template>
  <div class="home">
    <div class="background-image"></div>
    <button @click="goToForm">Start</button>
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
}

.background-image {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('/path/to/image.jpg'); /* Set your image path */
  background-size: cover;
  background-position: center;
  z-index: -1;
}

button {
  padding: 1rem 2rem;
  font-size: 1.5rem;
  cursor: pointer;
}
</style>
