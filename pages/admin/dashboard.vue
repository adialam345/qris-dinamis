<template>
  <div class="container mx-auto px-4 py-8">
    <div class="mb-8">
      <h1 class="text-3xl font-bold text-gray-900">Dashboard Admin</h1>
      <p class="text-gray-600 mt-2">Kelola data store dan monitor QRIS</p>
    </div>

    <!-- Stats Cards -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
      <div class="bg-white p-6 rounded-lg shadow">
        <div class="flex items-center">
          <div class="p-2 bg-blue-100 rounded-lg">
            <svg class="w-6 h-6 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"></path>
            </svg>
          </div>
          <div class="ml-4">
            <p class="text-sm font-medium text-gray-600">Total Store</p>
            <p class="text-2xl font-semibold text-gray-900">{{ stores.length }}</p>
          </div>
        </div>
      </div>

      <div class="bg-white p-6 rounded-lg shadow">
        <div class="flex items-center">
          <div class="p-2 bg-green-100 rounded-lg">
            <svg class="w-6 h-6 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
            </svg>
          </div>
          <div class="ml-4">
            <p class="text-sm font-medium text-gray-600">Store Aktif</p>
            <p class="text-2xl font-semibold text-gray-900">{{ activeStores }}</p>
          </div>
        </div>
      </div>

      <div class="bg-white p-6 rounded-lg shadow">
        <div class="flex items-center">
          <div class="p-2 bg-yellow-100 rounded-lg">
            <svg class="w-6 h-6 text-yellow-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1"></path>
            </svg>
          </div>
          <div class="ml-4">
            <p class="text-sm font-medium text-gray-600">Total QR Generated</p>
            <p class="text-2xl font-semibold text-gray-900">{{ totalQRGenerated }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Store Management -->
    <div class="bg-white rounded-lg shadow">
      <div class="px-6 py-4 border-b border-gray-200">
        <div class="flex justify-between items-center">
          <h2 class="text-xl font-semibold text-gray-900">Daftar Store</h2>
          <button
            @click="showAddStoreModal = true"
            class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-md text-sm font-medium"
          >
            Tambah Store
          </button>
        </div>
      </div>

      <div class="overflow-x-auto">
        <table class="min-w-full divide-y divide-gray-200">
          <thead class="bg-gray-50">
            <tr>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Store</th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Email</th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">QR Generated</th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
            </tr>
          </thead>
          <tbody class="bg-white divide-y divide-gray-200">
            <tr v-for="store in stores" :key="store.id">
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="text-sm font-medium text-gray-900">{{ store.name }}</div>
                <div class="text-sm text-gray-500">{{ store.address }}</div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ store.email }}</td>
              <td class="px-6 py-4 whitespace-nowrap">
                <span :class="[
                  'inline-flex px-2 py-1 text-xs font-semibold rounded-full',
                  store.status === 'active' ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'
                ]">
                  {{ store.status === 'active' ? 'Aktif' : 'Nonaktif' }}
                </span>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ store.qrGenerated || 0 }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                <button
                  @click="editStore(store)"
                  class="text-blue-600 hover:text-blue-900 mr-3"
                >
                  Edit
                </button>
                <button
                  @click="deleteStore(store.id)"
                  class="text-red-600 hover:text-red-900"
                >
                  Hapus
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Add/Edit Store Modal -->
    <div v-if="showAddStoreModal || showEditStoreModal" class="fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full z-50">
      <div class="relative top-20 mx-auto p-5 border w-96 shadow-lg rounded-md bg-white">
        <div class="mt-3">
          <h3 class="text-lg font-medium text-gray-900 mb-4">
            {{ showAddStoreModal ? 'Tambah Store' : 'Edit Store' }}
          </h3>
          
          <form @submit.prevent="saveStore">
            <div class="space-y-4">
              <div>
                <label class="block text-sm font-medium text-gray-700">Nama Store</label>
                <input
                  v-model="storeForm.name"
                  type="text"
                  required
                  class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500"
                />
              </div>
              
              <div>
                <label class="block text-sm font-medium text-gray-700">Email</label>
                <input
                  v-model="storeForm.email"
                  type="email"
                  required
                  class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500"
                />
              </div>
              
              <div>
                <label class="block text-sm font-medium text-gray-700">Alamat</label>
                <textarea
                  v-model="storeForm.address"
                  required
                  rows="3"
                  class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500"
                ></textarea>
              </div>
              
              <div>
                <label class="block text-sm font-medium text-gray-700">Status</label>
                <select
                  v-model="storeForm.status"
                  class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500"
                >
                  <option value="active">Aktif</option>
                  <option value="inactive">Nonaktif</option>
                </select>
              </div>
            </div>
            
            <div class="flex justify-end space-x-3 mt-6">
              <button
                type="button"
                @click="closeModal"
                class="px-4 py-2 border border-gray-300 rounded-md text-sm font-medium text-gray-700 hover:bg-gray-50"
              >
                Batal
              </button>
              <button
                type="submit"
                class="px-4 py-2 bg-blue-600 border border-transparent rounded-md text-sm font-medium text-white hover:bg-blue-700"
              >
                Simpan
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, inject, onMounted } from 'vue'

// Inject global functions
const showNotification = inject('showNotification')

// State
const stores = ref([])
const showAddStoreModal = ref(false)
const showEditStoreModal = ref(false)
const storeForm = ref({
  id: null,
  name: '',
  email: '',
  address: '',
  status: 'active'
})

// Computed
const activeStores = computed(() => stores.value.filter(store => store.status === 'active').length)
const totalQRGenerated = computed(() => stores.value.reduce((total, store) => total + (store.qrGenerated || 0), 0))

// Methods
const loadStores = () => {
  const savedStores = localStorage.getItem('stores')
  if (savedStores) {
    stores.value = JSON.parse(savedStores)
  } else {
    // Demo data
    stores.value = [
      {
        id: 1,
        name: 'Store ABC',
        email: 'store@qris.com',
        address: 'Jl. Contoh No. 123, Jakarta',
        status: 'active',
        qrGenerated: 15
      },
      {
        id: 2,
        name: 'Toko XYZ',
        email: 'toko@qris.com',
        address: 'Jl. Demo No. 456, Bandung',
        status: 'active',
        qrGenerated: 8
      }
    ]
    saveStores()
  }
}

const saveStores = () => {
  localStorage.setItem('stores', JSON.stringify(stores.value))
}

const editStore = (store) => {
  storeForm.value = { ...store }
  showEditStoreModal.value = true
}

const deleteStore = (id) => {
  if (confirm('Apakah Anda yakin ingin menghapus store ini?')) {
    stores.value = stores.value.filter(store => store.id !== id)
    saveStores()
    showNotification('Store berhasil dihapus', 'success')
  }
}

const saveStore = () => {
  if (showAddStoreModal.value) {
    // Add new store
    const newStore = {
      ...storeForm.value,
      id: Date.now(),
      qrGenerated: 0
    }
    stores.value.push(newStore)
    showNotification('Store berhasil ditambahkan', 'success')
  } else {
    // Edit existing store
    const index = stores.value.findIndex(store => store.id === storeForm.value.id)
    if (index !== -1) {
      stores.value[index] = { ...storeForm.value }
      showNotification('Store berhasil diupdate', 'success')
    }
  }
  
  saveStores()
  closeModal()
}

const closeModal = () => {
  showAddStoreModal.value = false
  showEditStoreModal.value = false
  storeForm.value = {
    id: null,
    name: '',
    email: '',
    address: '',
    status: 'active'
  }
}

// Check authentication
onMounted(() => {
  const currentUser = localStorage.getItem('currentUser')
  if (!currentUser) {
    navigateTo('/login')
    return
  }
  
  const user = JSON.parse(currentUser)
  if (user.role !== 'admin') {
    navigateTo('/login')
    return
  }
  
  loadStores()
})
</script>
