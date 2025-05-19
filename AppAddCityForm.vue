<script setup>
import { ref } from 'vue'
import { defineEmits } from 'vue'
import cityApi from '@/api/city'

const emit = defineEmits(['city-added'])

const form = ref({
  name: '',
  description: '',
  country: '',
  settledYear: null,
  consolidatedYear: null,
  population: null,
  zipCode: null,
  imageUrl: '',
})

const successMessage = ref('')
const errorMessage = ref('')

const handleSubmit = async () => {
  try {
    await cityApi.createCity(form.value)
    successMessage.value = 'Grad je uspješno dodan!'
    errorMessage.value = ''

    emit('city-added')

    form.value = {
      name: '',
      description: '',
      country: '',
      settledYear: null,
      consolidatedYear: null,
      population: null,
      zipCode: null,
      imageUrl: '',
    }
  } catch (err) {
    console.error('Greška prilikom slanja zahtjeva:', err)
    errorMessage.value = 'Došlo je do greške prilikom dodavanja grada.'
    successMessage.value = ''
  }
}
</script>

<template>
  <div class="form-wrapper">
    <h2>Dodaj novi grad</h2>
    <form @submit.prevent="handleSubmit" class="city-form">
      <label>
        Naziv:
        <input v-model="form.name" required />
      </label>

      <label>
        Opis:
        <textarea v-model="form.description" required></textarea>
      </label>

      <label>
        Država:
        <input v-model="form.country" required />
      </label>

      <label>
        Godina osnutka:
        <input type="number" v-model.number="form.settledYear" required />
      </label>

      <label>
        Godina konsolidacije:
        <input type="number" v-model.number="form.consolidatedYear" required />
      </label>

      <label>
        Broj stanovnika:
        <input type="number" v-model.number="form.population" required />
      </label>

      <label>
        Poštanski broj:
        <input type="number" v-model.number="form.zipCode" required />
      </label>

      <label>
        URL slike:
        <input v-model="form.imageUrl" required />
      </label>

      <button type="submit">Dodaj grad</button>
    </form>

    <p v-if="successMessage" class="success">{{ successMessage }}</p>
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </div>
</template>

<style scoped>
.form-wrapper {
  max-width: 500px;
  margin: 2rem auto;
  padding: 1rem;
}

.city-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

input,
textarea {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
}

button {
  padding: 0.75rem 1.5rem;
  background-color: #2563eb;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  font-size: 1rem;
  transition:
    background-color 0.3s ease,
    box-shadow 0.3s ease;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.success {
  color: green;
  margin-top: 1rem;
}

.error {
  color: red;
  margin-top: 1rem;
}
</style>
