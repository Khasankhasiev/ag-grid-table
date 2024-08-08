<template>
  <div>
    <h2>Лог редактирования</h2>
    <ul>
      <li v-for="(log, index) in logs" :key="index">{{ log }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const logs = ref([])

const addLog = (message) => {
  logs.value.push(message)
}

window.addEventListener('log-event', (event) => {
  addLog(event.detail)
})
</script>

<style scoped>
h2 {
  text-align: center;
}
</style>
