<template>
  <main class="app">
    <h1>Vue 3 Modal Component Demo</h1>

    <button class="btn primary" @click="open = true">
      Open Modal
    </button>

    <!-- Reusable Modal Component -->
    <Modal
      v-model:show="open"
      size="lg"
      position="top"
      showCloseButton
      animation="slide-up"
      rounded="16px"
      width="600px"
    >
      <template #header>
        <h2>Custom Modal</h2>
      </template>

      <template #default>
        <p>This modal demonstrates all custom props.</p>
      </template>

      <template #footer>
        <button class="btn" @click="open = false">Close</button>
      </template>
    </Modal>
    <!-- <Modal
      v-model:show="openBlur"
      animation="scale"
      blurOverlay
      showCloseButton
    >
      <template #header>
        <h2>Spring Animated Modal</h2>
      </template>

      <p>This modal uses a blurred background and spring motion.</p>

      <template #footer>
        <button class="btn" @click="openBlur = false">Close</button>
      </template>
    </Modal> -->
    <!-- <Modal
      v-model:show="panelRight"
      animation="slide-right"
      panelMode
      showCloseButton
    >
      <template #header>
        <h2>Right Panel</h2>
      </template>

      <p>A mobile-friendly slide-in panel.</p>

      <template #footer>
        <button class="btn primary" @click="panelRight = false">Done</button>
      </template>
    </Modal> -->
    <!-- <Modal
      v-model:show="bottomSheet"
      animation="slide-up"
      panelMode
    >
      <template #header>
        <h2>Bottom Sheet</h2>
      </template>

      <p>This bottom sheet slides up from the bottom of the screen.</p>

      <template #footer>
        <button class="btn" @click="bottomSheet = false">Close</button>
      </template>
    </Modal> -->
  </main>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import Modal from './components/Modal.vue'

const open = ref(false)

const confirmAction = () => {
  alert('Confirmed action!')
  open.value = false
}
</script>

<style>
.app {
  padding: 2rem;
  font-family: system-ui, sans-serif;
  max-width: 800px;
  margin: 0 auto;
}

h1 {
  margin-bottom: 1.5rem;
}

.modal-title {
  margin: 0 0 0.5rem 0;
}

/* Buttons */
.btn {
  padding: 0.6rem 1.2rem;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  margin-left: 0.5rem;
  background: #ddd;
  transition: background 0.2s;
}

.btn:hover {
  background: #ccc;
}

.btn.primary {
  background: #3b82f6;
  color: white;
}

.btn.primary:hover {
  background: #2563eb;
}
</style>
