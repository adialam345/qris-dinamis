<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
    <div class="container mx-auto px-4 py-16">
      <div class="text-center">
        <h1 class="text-5xl font-bold text-gray-900 mb-6">
          QRIS Dinamis
        </h1>
        <p class="text-xl text-gray-600 mb-8 max-w-2xl mx-auto">
          Solusi lengkap untuk generate QRIS dinamis dengan nominal yang dapat disesuaikan. 
          Kelola store dan buat QR pembayaran dengan mudah.
        </p>
        
        <div class="flex justify-center space-x-4">
          <button
            @click="navigateTo('/login')"
            class="bg-blue-600 hover:bg-blue-700 text-white px-8 py-3 rounded-lg text-lg font-medium transition-colors"
          >
            Mulai Sekarang
          </button>
        </div>
      </div>

      <!-- Features -->
      <div class="mt-20 grid grid-cols-1 md:grid-cols-3 gap-8">
        <div class="bg-white p-6 rounded-lg shadow-lg">
          <div class="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center mb-4">
            <svg class="w-6 h-6 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"></path>
            </svg>
          </div>
          <h3 class="text-lg font-semibold text-gray-900 mb-2">Kelola Store</h3>
          <p class="text-gray-600">Admin dapat mengelola data store dengan fitur CRUD lengkap</p>
        </div>

        <div class="bg-white p-6 rounded-lg shadow-lg">
          <div class="w-12 h-12 bg-green-100 rounded-lg flex items-center justify-center mb-4">
            <svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1"></path>
            </svg>
          </div>
          <h3 class="text-lg font-semibold text-gray-900 mb-2">QRIS Dinamis</h3>
          <p class="text-gray-600">Generate QR dengan nominal yang dapat disesuaikan sesuai kebutuhan</p>
        </div>

        <div class="bg-white p-6 rounded-lg shadow-lg">
          <div class="w-12 h-12 bg-yellow-100 rounded-lg flex items-center justify-center mb-4">
            <svg class="w-6 h-6 text-yellow-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v10a2 2 0 002 2h8a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"></path>
            </svg>
          </div>
          <h3 class="text-lg font-semibold text-gray-900 mb-2">History Tracking</h3>
          <p class="text-gray-600">Lihat dan kelola history QR yang pernah di-generate</p>
        </div>
      </div>

      <!-- Demo Info -->
      <div class="mt-16 bg-white rounded-lg shadow-lg p-8">
        <h2 class="text-2xl font-bold text-gray-900 text-center mb-6">Demo Credentials</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <div class="text-center">
            <h3 class="text-lg font-semibold text-blue-600 mb-3">Admin</h3>
            <p class="text-gray-600 mb-2">Email: admin@qris.com</p>
            <p class="text-gray-600">Password: admin123</p>
            <p class="text-sm text-gray-500 mt-2">Kelola store dan monitor QRIS</p>
          </div>
          <div class="text-center">
            <h3 class="text-lg font-semibold text-green-600 mb-3">Store</h3>
            <p class="text-gray-600 mb-2">Email: store@qris.com</p>
            <p class="text-gray-600">Password: store123</p>
            <p class="text-sm text-gray-500 mt-2">Upload template dan generate QR</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// Redirect to login if user is not authenticated
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
