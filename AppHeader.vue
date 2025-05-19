<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faMagnifyingGlass, faBell } from '@fortawesome/free-solid-svg-icons'
import AppProfileButton from './AppProfileButton.vue'
import { ref } from 'vue'

const showBackground = ref(true)
</script>

<template>
  <header
    class="flex items-center justify-between px-md header"
    :class="{ 'header--background': showBackground }"
  >
    <div>
      <h4>Leksikon mojih putovanja</h4>
    </div>
    <div class="flex items-right gap-md">
      <FontAwesomeIcon :icon="faMagnifyingGlass" class="icon" style="font-size: 20px" />
      <FontAwesomeIcon :icon="faBell" class="icon" style="font-size: 20px" />
      <div>
        <AppProfileButton />
      </div>
    </div>
  </header>
</template>

<style scoped>
.header {
  position: fixed;
  top: 20px;
  width: 100%;
  z-index: 40;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0));
}

.header--background {
  background-color: #18181b;
}

.header h4 {
  font-family: 'Great Vibes', cursive;
  font-size: 2rem;
  color: white;
  margin: 0;
}
.icon {
  color: #fff;
  font-size: 13px;
}
</style>
