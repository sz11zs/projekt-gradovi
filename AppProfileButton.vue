<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faArrowDown, faArrowUp } from '@fortawesome/free-solid-svg-icons'
import { useUserStore } from '@/stores/user'
import { useRouter } from 'vue-router'

const router = useRouter()
const userStore = useUserStore()

const isDropdownOpen = ref(false)

const toggleDropdown = () => {
  isDropdownOpen.value = !isDropdownOpen.value
}

const logout = () => {
  localStorage.removeItem('token')
  router.push({ name: 'AuthPage' })
}

// klik van dropdown-a da se zatvori
const closeDropdownOnClickOutside = (event) => {
  if (!event.target.closest('.dropdown')) {
    isDropdownOpen.value = false
  }
}

onMounted(() => {
  window.addEventListener('click', closeDropdownOnClickOutside)
})

onBeforeUnmount(() => {
  window.removeEventListener('click', closeDropdownOnClickOutside)
})
</script>

<template>
  <div class="dropdown" @click.stop>
    <button class="btn" @click="toggleDropdown">
      <img src="@/assets/img/profile.jpg" alt="Profil" class="profile" />
      <FontAwesomeIcon :icon="isDropdownOpen ? faArrowUp : faArrowDown" class="icon" />
    </button>

    <div v-if="isDropdownOpen" class="dropdown-menu">
      <div class="dropdown-item">{{ userStore.currentUser?.firstName || 'Korisnik' }}</div>
      <div class="dropdown-item logout" @click="logout">Logout</div>
    </div>
  </div>
</template>

<style scoped>
.dropdown {
  position: relative;
  display: inline-block;
}

.btn {
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 5px;
}

.profile {
  height: 35px;
  border-radius: 50%;
}

.icon {
  color: white;
  font-size: 13px;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  right: 0;
  background-color: #1f2937;
  color: white;
  border: 1px solid #444;
  border-radius: 5px;
  min-width: 150px;
  margin-top: 5px;
  z-index: 1000;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.dropdown-item {
  padding: 10px 15px;
  cursor: pointer;
}

.dropdown-item:hover {
  background-color: #374151;
}

.logout {
  border-top: 1px solid #333;
}
</style>
