<template>
  <nav class="bg-white shadow-lg">
    <div class="max-w-7xl mx-auto px-4">
      <div class="flex justify-between h-16">
        <div class="flex items-center">
          <h1 class="text-xl font-bold text-gray-800">QRIS Dinamis</h1>
        </div>
        
        <div class="flex items-center space-x-4">
          <span class="text-gray-600">Selamat datang, {{ user.name }}</span>
          <span class="px-3 py-1 bg-blue-100 text-blue-800 rounded-full text-sm">
            {{ user.role === 'admin' ? 'Admin' : 'Store' }}
          </span>
          <button 
            @click="$emit('logout')"
            class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-md text-sm font-medium transition-colors"
          >
            Logout
          </button>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
defineProps({
  user: {
    type: Object,
    required: true
  }
})

defineEmits(['logout'])
</script>
