<script setup lang="ts">
import { ref } from "vue";
import JSConfetti from 'js-confetti'
import type { Ref } from "vue";

type ApiResponse = { status: string; message: string };

const jsConfetti = new JSConfetti();


const imageSrc: Ref<null | ApiResponse["message"]> = ref("");
const isButtonDisabled = ref(false);

async function fetchKitten() {
  imageSrc.value = null;
  isButtonDisabled.value = true;
  try {
    const res = await fetch("https://dog.ceo/api/breeds/image/random");
    const imgLink: ApiResponse = await res.json();
    imageSrc.value = imgLink.message;
    jsConfetti.addConfetti({
      confettiRadius: 1000000,
      emojiSize: 200,
      confettiNumber: 20,
      emojis: ['🐶', '🦴']
    });
  } catch (error) {
    console.error("Error!");
  } finally {
    isButtonDisabled.value = false;
  }
}
</script>

<template>
  <div class="container">
    <div v-if="imageSrc === null" class="loading">
      <p>Loading...</p>
    </div>
    <div v-else-if="imageSrc === ''" class="empty"></div>
    <img v-else v-bind:src="imageSrc" alt="Beautiful cute dog" />
    <button @click="fetchKitten" :disabled="isButtonDisabled">CLICK ME</button>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  max-width: 600px;
  margin: 0 auto;
  align-items: center;
  justify-content: center;
  height: 100%;
  gap: 20px;
}

.loading,
img {
  height: 300px;
}

button {
  padding: 10px;
  background-color: black;
  font-weight: bold;
}

.loading {
  display: flex;
  align-items: center;
  justify-content: center;
}

img {
  width: 100%;
  object-fit: contain;
}

@media (max-width: 1024px) {

  .loading,
  img {
    height: 150px;
  }

  img {
    width: 100%;
  }
}
</style>
