<script setup>
import { onMounted, ref } from 'vue'
import listApi from '@/api/list'
import cityApi from '@/api/city'

const cities = ref([])
const error = ref('')
const selectedCity = ref(null)
const isModalOpen = ref(false)

const deleteCity = async () => {
  if (!selectedCity.value) return

  const confirmed = confirm(`Jesi siguran da želiš obrisati grad "${selectedCity.value.name}"?`)
  if (!confirmed) return

  try {
    await cityApi.deleteCity(selectedCity.value.id)
    // Zatvori modal i ponovo dohvatiti gradove
    closeModal()
  } catch (err) {
    console.error('Greška prilikom brisanja grada:', err)
    alert('Došlo je do greške pri brisanju grada.')
  }
}

const openModal = async (city) => {
  try {
    const fullCity = await cityApi.getCityById(city.id)
    console.log('FULL CITY:', fullCity)

    selectedCity.value = fullCity
    isModalOpen.value = true
  } catch {
    error.value = 'Ne mogu dohvatiti podatke o gradu.'
  }
}

const closeModal = () => {
  isModalOpen.value = false
  selectedCity.value = null
  selectedCity.value = null
}

onMounted(async () => {
  try {
    cities.value = await listApi.getAllCities('name', 'ASC')
  } catch {
    error.value = 'Niste prijavljeni ili je došlo do greške.'
  }
})
</script>

<template>
  <div class="city-list">
    <div v-if="error">{{ error }}</div>

    <ul v-else>
      <li v-for="city in cities" :key="city.id">
        <img
          :src="city.imageUrl"
          alt="Grad"
          width="100"
          @click="openModal(city)"
          style="cursor: pointer"
        />
        <div>
          <h3>{{ city.name }}</h3>
        </div>
      </li>
    </ul>

    <div v-if="selectedCity" class="modal-overlay" @click.self="closeModal">
      <div class="modal-content">
        <button class="close-btn" @click="closeModal">×</button>
        <h2>{{ selectedCity.name }}</h2>
        <img :src="selectedCity.imageUrl" alt="Slika grada" width="300" />
        <p><strong>Opis:</strong> {{ selectedCity.description }}</p>
        <p><strong>Država:</strong> {{ selectedCity.country }}</p>
        <p><strong>Godina osnutka:</strong> {{ selectedCity.settledYear }}</p>
        <p><strong>Godina konsolidacije:</strong> {{ selectedCity.consolidatedYear }}</p>
        <p><strong>Broj stanovnika:</strong> {{ selectedCity.population }}</p>
        <p><strong>Poštanski broj:</strong> {{ selectedCity.zipCode }}</p>

        <button class="delete-btn" @click="deleteCity(selectedCity.id)">Obriši</button>
        <button class="edit-btn" @click="isEditing = true">Uredi</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
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
  padding: 2rem;
  border-radius: 10px;
  width: 90%;
  max-width: 500px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  position: relative;
  max-height: 90vh;
  overflow-y: auto;
  animation: fadeInScale 0.3s ease;
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

/* Animacija za ulazak modala */
@keyframes fadeInScale {
  0% {
    opacity: 0;
    transform: scale(0.9);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}
.edit-btn {
  padding: 0.5rem 1rem;
  margin-right: 0.5rem;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}
.delete-btn {
  padding: 0.5rem 1rem;
  margin-right: 0.5rem;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.edit-btn {
  background-color: #2563eb;
  color: white;
}

.edit-btn:hover {
  background-color: #1d4ed8;
}

.delete-btn {
  background-color: #dc2626;
  color: white;
}

.delete-btn:hover {
  background-color: #b91c1c;
}
</style>
