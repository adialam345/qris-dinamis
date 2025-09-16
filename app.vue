<template>
  <div id="app">
    <!-- Navigation -->
    <Navbar v-if="isAuthenticated" :user="currentUser" @logout="handleLogout" />
    
    <!-- Main Content -->
    <main class="min-h-screen bg-gray-50">
      <NuxtPage />
    </main>
    
    <!-- Notification -->
    <Notification v-if="notification.show" :message="notification.message" :type="notification.type" @close="closeNotification" />
  </div>
</template>

<script setup>
import { ref, computed, onMounted, provide } from 'vue'

// Global state
const currentUser = ref(null)
const notification = ref({
  show: false,
  message: '',
  type: 'success'
})

// Computed
const isAuthenticated = computed(() => currentUser.value !== null)

// Methods
const handleLogout = () => {
  if (process.client) {
    localStorage.removeItem('currentUser')
  }
  currentUser.value = null
  navigateTo('/login')
  showNotification('Logout berhasil', 'success')
}

const showNotification = (message, type = 'success') => {
  notification.value = {
    show: true,
    message,
    type
  }
  setTimeout(() => {
    notification.value.show = false
  }, 3000)
}

const closeNotification = () => {
  notification.value.show = false
}

// Check authentication on mount
onMounted(() => {
  if (process.client) {
    const user = localStorage.getItem('currentUser')
    if (user) {
      currentUser.value = JSON.parse(user)
    }
  }
})

// Provide global functions
provide('currentUser', currentUser)
provide('showNotification', showNotification)
</script>
