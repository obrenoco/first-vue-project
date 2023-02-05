<script setup lang="ts">
import { ref } from "vue";
import type { Ref } from "vue";

type ApiResponse = { status: string; message: string };
const imageSrc: Ref<null | ApiResponse["message"]> = ref(null);

async function fetchKitten() {
  imageSrc.value = null;
  try {
    const res = await fetch("https://dog.ceo/api/breeds/image/random");
    const imgLink: ApiResponse = await res.json();
    imageSrc.value = imgLink.message;
  } catch (error) {
    console.error("Error!");
  }
}
</script>

<template>
  <div>
    <img v-if="imageSrc" v-bind:src="imageSrc" alt="Beautiful cute dog" />
    <p v-else>Loading...</p>
    <button @click="fetchKitten">CLICK ME</button>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
}

img {
  width: 100%;
  height: 500px;
}

@media (max-width: 1024px) {
  img {
    width: 100%;
    height: 100%;
  }
}
</style>
