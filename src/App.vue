<template>
  <div class="container">
    <AddUserForm />
    <UserTable />
    <Log />
  </div>
</template>

<script setup>
import AddUserForm from './components/AddUserForm.vue'
import UserTable from './components/UserTable.vue'
import Log from './components/Log.vue'
</script>

<style scoped>
.container {
  width: 100%;
  max-width: 1025px;
  margin: 0 auto;
}
</style>
