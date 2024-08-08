<template>
  <div>
    <h2>Добавить пользователя</h2>
    <form class="form" @submit.prevent="addUser">
      <input v-model="name" placeholder="Имя" required />
      <select v-model="gender" required>
        <option value="">Пол</option>
        <option value="male">Male</option>
        <option value="female">Female</option>
      </select>
      <input v-model="email" placeholder="Email" required />
      <input v-model="age" placeholder="Возраст" required type="number" />
      <button type="submit">Добавить</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const name = ref('')
const gender = ref('')
const email = ref('')
const age = ref('')

const addUser = () => {
  const newUser = {
    name: { first: name.value },
    gender: gender.value,
    email: email.value,
    dob: { age: age.value }
  }
  const event = new CustomEvent('row-added', { detail: newUser })
  window.dispatchEvent(event)
  name.value = ''
  gender.value = ''
  email.value = ''
  age.value = ''
}
</script>

<style scoped>
h2 {
  text-align: center;
}
.form {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
  justify-content: center;
}
</style>
