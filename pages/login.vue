<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-md w-full space-y-8">
      <div>
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
          Login QRIS Dinamis
        </h2>
        <p class="mt-2 text-center text-sm text-gray-600">
          Pilih role untuk login
        </p>
      </div>
      
      <div class="mt-8 space-y-6">
        <!-- Role Selection -->
        <div class="flex space-x-4">
          <button
            @click="selectedRole = 'admin'"
            :class="[
              'flex-1 py-2 px-4 border rounded-md font-medium',
              selectedRole === 'admin' 
                ? 'bg-blue-500 text-white border-blue-500' 
                : 'bg-white text-gray-700 border-gray-300 hover:bg-gray-50'
            ]"
          >
            Admin
          </button>
          <button
            @click="selectedRole = 'store'"
            :class="[
              'flex-1 py-2 px-4 border rounded-md font-medium',
              selectedRole === 'store' 
                ? 'bg-blue-500 text-white border-blue-500' 
                : 'bg-white text-gray-700 border-gray-300 hover:bg-gray-50'
            ]"
          >
            Store
          </button>
        </div>

        <!-- Login Form -->
        <form class="mt-8 space-y-6" @submit.prevent="handleLogin">
          <div class="rounded-md shadow-sm -space-y-px">
            <div>
              <label for="email" class="sr-only">Email</label>
              <input
                id="email"
                v-model="form.email"
                name="email"
                type="email"
                required
                class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-blue-500 focus:border-blue-500 focus:z-10 sm:text-sm"
                placeholder="Email address"
              />
            </div>
            <div>
              <label for="password" class="sr-only">Password</label>
              <input
                id="password"
                v-model="form.password"
                name="password"
                type="password"
                required
                class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-blue-500 focus:border-blue-500 focus:z-10 sm:text-sm"
                placeholder="Password"
              />
            </div>
          </div>

          <div>
            <button
              type="submit"
              class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500"
            >
              Login
            </button>
          </div>

          <!-- Demo Credentials -->
          <div class="mt-4 p-4 bg-gray-100 rounded-md">
            <h3 class="text-sm font-medium text-gray-700 mb-2">Demo Credentials:</h3>
            <div class="text-xs text-gray-600 space-y-1">
              <div><strong>Admin:</strong> admin@qris.com / admin123</div>
              <div><strong>Store:</strong> store@qris.com / store123</div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, inject } from 'vue'

// Inject global functions
const showNotification = inject('showNotification')

// State
const selectedRole = ref('admin')
const form = ref({
  email: '',
  password: ''
})

// Demo data
const demoUsers = [
  {
    id: 1,
    email: 'admin@qris.com',
    password: 'admin123',
    name: 'Admin QRIS',
    role: 'admin'
  },
  {
    id: 2,
    email: 'store@qris.com',
    password: 'store123',
    name: 'Store ABC',
    role: 'store',
    storeId: 1
  }
]

// Methods
const handleLogin = () => {
  const user = demoUsers.find(u => 
    u.email === form.value.email && 
    u.password === form.value.password &&
    u.role === selectedRole.value
  )

  if (user) {
    // Save user to localStorage
    localStorage.setItem('currentUser', JSON.stringify(user))
    
    // Redirect based on role
    if (user.role === 'admin') {
      navigateTo('/admin/dashboard')
    } else {
      navigateTo('/store/dashboard')
    }
    
    showNotification('Login berhasil!', 'success')
  } else {
    showNotification('Email atau password salah!', 'error')
  }
}

// Redirect if already logged in
onMounted(() => {
  const currentUser = localStorage.getItem('currentUser')
  if (currentUser) {
    const user = JSON.parse(currentUser)
    if (user.role === 'admin') {
      navigateTo('/admin/dashboard')
    } else {
      navigateTo('/store/dashboard')
    }
  }
})
</script>
