<template>
  <button class="btn primary" @click="showForm = true">
    Add User
  </button>

  <Modal
    v-model:show="showForm"
    size="md"
    animation="fade"
    showCloseButton
  >
    <template #header>
      <h2>Add New User</h2>
    </template>

    <form @submit.prevent="submitForm">
      <label>Name</label>
      <input v-model="user.name" required />

      <label>Email</label>
      <input v-model="user.email" type="email" required />

      <label>Role</label>
      <select v-model="user.role" required>
        <option>User</option>
        <option>Admin</option>
      </select>

      <template #footer>
        <button type="button" class="btn" @click="showForm = false">
          Cancel
        </button>
        <button type="submit" class="btn primary">
          Save
        </button>
      </template>
    </form>
  </Modal>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const showForm = ref(false)
const user = ref({ name: '', email: '', role: 'User' })

const submitForm = () => {
  console.log('User saved:', user.value)
  showForm.value = false
}
</script>

<style>
form input, form select {
  width: 100%;
  margin-bottom: 1rem;
  padding: 0.5rem;
}
</style>
