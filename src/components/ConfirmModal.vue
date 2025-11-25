<template>
  <button class="btn warn" @click="showConfirm = true">
    Delete Item
  </button>

  <Modal
    v-model:show="showConfirm"
    animation="scale"
    showCloseButton
    roleType="alertdialog"
  >
    <template #header>
      <h2>Confirm Action</h2>
    </template>

    <p>Are you sure you want to delete this item? This action cannot be undone.</p>

    <template #footer>
      <button class="btn" @click="showConfirm = false">Cancel</button>
      <button class="btn danger" @click="confirmDelete">
        Yes, Delete
      </button>
    </template>
  </Modal>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const showConfirm = ref(false)
const confirmDelete = () => {
  alert('Item deleted.')
  showConfirm.value = false
}
</script>

<style>
.btn.warn { background: #f59e0b; color: white; }
.btn.danger { background: #dc2626; color: white; }
</style>