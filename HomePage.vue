<script setup>
import { ref } from 'vue'
import AppHeader from '@/components/AppHeader.vue'
import AppAddCityForm from '@/components/AppAddCityForm.vue'
import AppCityList from '@/components/AppCityList.vue'

const isAddCityModalOpen = ref(false)

const openAddCityModal = () => {
  isAddCityModalOpen.value = true // Otvoriti modal
}

const closeAddCityModal = () => {
  isAddCityModalOpen.value = false // Zatvoriti modal
}
</script>

<template>
  <AppHeader />
  <div class="page">
    <div class="panel">
      <AppCityList />
      <!-- Dugme koje otvara modal -->
      <button @click="openAddCityModal" class="add-btn">Dodaj grad</button>

      <!-- Modal za dodavanje grada -->
      <div v-if="isAddCityModalOpen" class="modal-overlay" @click.self="closeAddCityModal">
        <div class="modal-content">
          <button class="close-btn" @click="closeAddCityModal">×</button>
          <!-- Form za dodavanje grada -->
          <AppAddCityForm @city-added="closeAddCityModal" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: var(--gradient-bg), url('../assets/img/pozadina1.jpg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.panel {
  width: 1200px;
  height: 600px;
  background-color: rgba(255, 255, 255, 0.5);
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
  overflow-y: auto;
  backdrop-filter: blur(6px);
  margin: auto;
}

.add-btn {
  position: absolute;
  bottom: 20px;
  right: 20px;
  padding: 10px 20px;
  background-color: #3d92a7;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: #3d92a7;
  background-color:;
  padding: 2rem;
  border-radius: 10px;
  width: 90%;
  max-width: 600px;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  position: relative;
}

.close-btn {
  position: absolute;
  top: 0.5rem;
  right: 1rem;
  background: transparent;
  border: none;
  font-size: 2rem;
  cursor: pointer;
  line-height: 1;
}
</style>
