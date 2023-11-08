<template>
  <div>
    <span>Multiline message is:</span>
    <p style="white-space: pre-line">{{ text_value }}</p>
    <textarea v-model="text_value" placeholder="add multiple lines"></textarea>
    <button @click="process_text">Procesar texto</button>
    <img :src="img_url" v-if="img_url" alt="AI Image" />
  </div>
</template>

<script setup>
import { ref } from "vue";

const text_value = ref("");
const img_url = ref(null);

async function query(data) {
  const response = await fetch("https://api-inference.huggingface.co/models/stabilityai/stable-diffusion-2-1", {
    headers: { Authorization: "Bearer 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" },
    method: "POST",
    body: JSON.stringify(data),
  });

  if (response.ok) {
    const blob = await response.blob();
    // Create a URL for the blob (assuming it's an image)
    img_url.value = URL.createObjectURL(blob);
  } else {
    // Handle error here
    console.error("Failed to fetch image");
  }
}

const process_text = () => {
  query({ inputs: text_value.value }).then(() => {
    // Handle success, URL of the image is set in img_url
  });
};
</script>

<style scoped></style>
