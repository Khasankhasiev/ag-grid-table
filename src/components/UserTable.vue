<template>
  <div>
    <ag-grid-vue
      style="height: 400px"
      class="ag-theme-alpine"
      :rowData="rowData"
      :columnDefs="columnDefs"
      @grid-ready="onGridReady"
    ></ag-grid-vue>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { AgGridVue } from 'ag-grid-vue3'
import axios from 'axios'

const gridApi = ref(null)
const columnDefs = ref([
  { headerName: 'Имя', field: 'name.first' },
  { headerName: 'Пол', field: 'gender' },
  { headerName: 'Email', field: 'email' },
  { headerName: 'Возраст', field: 'dob.age' },
  {
    headerName: 'Действие',
    cellRenderer: (params) => {
      const button = document.createElement('button')
      button.innerText = 'Удалить'
      button.addEventListener('click', () => {
        const event = new CustomEvent('row-removed', { detail: params.data })
        window.dispatchEvent(event)
      })
      return button
    }
  }
])

const rowData = ref([])

const fetchData = async () => {
  try {
    const response = await axios.get('https://randomuser.me/api/?results=5')
    rowData.value = response.data.results
  } catch (error) {
    console.error('Ошибка при получении пользователей:', error)
    const event = new CustomEvent('log-event', { detail: 'Ошибка при получении пользователей' })
    window.dispatchEvent(event)
  }
}

const onGridReady = (params) => {
  gridApi.value = params.api
  fetchData()
}

window.addEventListener('row-removed', (event) => {
  gridApi.value.applyTransaction({ remove: [event.detail] })
  const logEvent = new CustomEvent('log-event', {
    detail: `Удален пользователь ${event.detail.name.first}`
  })
  window.dispatchEvent(logEvent)
})

window.addEventListener('row-added', (event) => {
  gridApi.value.applyTransaction({ add: [event.detail] })
  const logEvent = new CustomEvent('log-event', {
    detail: `Добавлен пользователь ${event.detail.name.first}`
  })
  window.dispatchEvent(logEvent)
})
</script>
