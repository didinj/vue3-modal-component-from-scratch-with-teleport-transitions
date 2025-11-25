<template>
  <div class="gallery">
    <img
      v-for="img in images"
      :key="img"
      :src="img"
      @click="openImage(img)"
    />
  </div>

  <Modal
    v-model:show="showImage"
    animation="fade"
    size="full"
    blurOverlay
    showCloseButton
  >
    <img :src="activeImage" class="lightbox-img" />
  </Modal>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const images = [
  '/img/pic1.jpg',
  '/img/pic2.jpg',
  '/img/pic3.jpg',
]

const showImage = ref(false)
const activeImage = ref('')

const openImage = (src: string) => {
  activeImage.value = src
  showImage.value = true
}
</script>

<style>
.gallery {
  display: flex;
  gap: 10px;
}
.gallery img {
  width: 100px;
  height: 80px;
  cursor: pointer;
  object-fit: cover;
}
.lightbox-img {
  width: 100%;
  border-radius: 8px;
}
</style>
