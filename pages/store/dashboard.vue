<template>
  <div class="container mx-auto px-4 py-8">
    <div class="mb-8">
      <h1 class="text-3xl font-bold text-gray-900">Dashboard Store</h1>
      <p class="text-gray-600 mt-2">Kelola QRIS dan generate QR dinamis</p>
    </div>

    <!-- Stats Cards -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
      <div class="bg-white p-6 rounded-lg shadow">
        <div class="flex items-center">
          <div class="p-2 bg-blue-100 rounded-lg">
            <svg class="w-6 h-6 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1"></path>
            </svg>
          </div>
          <div class="ml-4">
            <p class="text-sm font-medium text-gray-600">QR Generated</p>
            <p class="text-2xl font-semibold text-gray-900">{{ qrHistory.length }}</p>
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
            <p class="text-sm font-medium text-gray-600">QRIS Template</p>
            <p class="text-2xl font-semibold text-gray-900">{{ hasQRISTemplate ? 'Ada' : 'Belum Ada' }}</p>
          </div>
        </div>
      </div>

      <div class="bg-white p-6 rounded-lg shadow">
        <div class="flex items-center">
          <div class="p-2 bg-yellow-100 rounded-lg">
            <svg class="w-6 h-6 text-yellow-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
            </svg>
          </div>
          <div class="ml-4">
            <p class="text-sm font-medium text-gray-600">Terakhir Generate</p>
            <p class="text-2xl font-semibold text-gray-900">{{ lastGenerated || 'Belum Ada' }}</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Store Configuration -->
    <div class="bg-white rounded-lg shadow mb-8">
      <div class="px-6 py-4 border-b border-gray-200">
        <h2 class="text-xl font-semibold text-gray-900">Konfigurasi Store</h2>
        <p class="text-gray-600 text-sm mt-1">
          <span v-if="!storeConfig.merchantId">Upload template QRIS untuk mengekstrak Merchant ID</span>
          <span v-else>Merchant ID berhasil diekstrak dari template QRIS</span>
        </p>
      </div>
      
      <div class="p-6">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-2">Merchant ID</label>
            <input
              v-model="storeConfig.merchantId"
              type="text"
              placeholder="Masukkan Merchant ID"
              class="w-full border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500"
            />
            <p class="text-xs text-gray-500 mt-1">
              <span v-if="!storeConfig.merchantId">Upload template QRIS untuk mengekstrak Merchant ID</span>
              <span v-else class="text-green-600">✓ Diekstrak dari template QRIS</span>
            </p>
          </div>
          
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-2">Kategori Merchant</label>
            <select
              v-model="storeConfig.category"
              class="w-full border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500"
            >
              <option value="5812">Restoran</option>
              <option value="5411">Supermarket</option>
              <option value="5999">Toko Umum</option>
              <option value="5814">Fast Food</option>
              <option value="5311">Department Store</option>
              <option value="5993">Toko Kue</option>
            </select>
          </div>
        </div>
        
        <div class="mt-4">
          <div v-if="!storeConfig.merchantId" class="mb-4 p-3 bg-yellow-50 border border-yellow-200 rounded-lg">
            <div class="flex">
              <div class="flex-shrink-0">
                <svg class="h-5 w-5 text-yellow-400" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M8.257 3.099c.765-1.36 2.722-1.36 3.486 0l5.58 9.92c.75 1.334-.213 2.98-1.742 2.98H4.42c-1.53 0-2.493-1.646-1.743-2.98l5.58-9.92zM11 13a1 1 0 11-2 0 1 1 0 012 0zm-1-8a1 1 0 00-1 1v3a1 1 0 002 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </div>
              <div class="ml-3">
                <h3 class="text-sm font-medium text-yellow-800">Merchant ID Belum Tersedia</h3>
                <div class="mt-2 text-sm text-yellow-700">
                  <p>Upload template QRIS statis terlebih dahulu untuk mengekstrak Merchant ID secara otomatis.</p>
                </div>
              </div>
            </div>
          </div>
          
          <div v-else class="mb-4 p-3 bg-green-50 border border-green-200 rounded-lg">
            <div class="flex">
              <div class="flex-shrink-0">
                <svg class="h-5 w-5 text-green-400" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
              </div>
              <div class="ml-3">
                <h3 class="text-sm font-medium text-green-800">Merchant ID Tersedia</h3>
                <div class="mt-2 text-sm text-green-700">
                  <p>Merchant ID berhasil diekstrak dari template QRIS. Anda dapat generate QR dinamis.</p>
                </div>
              </div>
            </div>
          </div>
          
          <button
            @click="saveStoreConfig"
            :disabled="!storeConfig.merchantId"
            class="bg-blue-600 hover:bg-blue-700 disabled:bg-gray-400 text-white px-4 py-2 rounded-md text-sm font-medium"
          >
            Simpan Konfigurasi
          </button>
        </div>
      </div>
    </div>

    <!-- QRIS Template Upload -->
    <div class="bg-white rounded-lg shadow mb-8">
      <div class="px-6 py-4 border-b border-gray-200">
        <h2 class="text-xl font-semibold text-gray-900">QRIS Template</h2>
        <p class="text-gray-600 text-sm mt-1">Upload template QRIS statis untuk digunakan sebagai dasar generate QR dinamis</p>
      </div>
      
      <div class="p-6">
        <div v-if="!hasQRISTemplate" class="border-2 border-dashed border-gray-300 rounded-lg p-6 text-center">
          <svg class="mx-auto h-12 w-12 text-gray-400" stroke="currentColor" fill="none" viewBox="0 0 48 48">
            <path d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
          </svg>
          <div class="mt-4">
            <label for="file-upload" class="cursor-pointer">
              <span class="mt-2 block text-sm font-medium text-gray-900">Upload QRIS Template</span>
              <span class="mt-1 block text-sm text-gray-500">PNG, JPG atau data QR</span>
            </label>
            <input
              id="file-upload"
              ref="fileInput"
              type="file"
              accept="image/*,.txt"
              @change="handleFileUpload"
              class="sr-only"
            />
          </div>
          <div class="mt-4">
            <button
              @click="$refs.fileInput.click()"
              class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-md text-sm font-medium"
            >
              Pilih File
            </button>
          </div>
        </div>

        <div v-else class="space-y-4">
          <div class="flex items-center justify-between p-4 bg-gray-50 rounded-lg">
            <div class="flex items-center">
              <svg class="w-8 h-8 text-green-500 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
              </svg>
              <div>
                <p class="text-sm font-medium text-gray-900">{{ qrisTemplate.name }}</p>
                <p class="text-xs text-gray-500">Uploaded: {{ formatDate(qrisTemplate.uploadedAt) }}</p>
                <div v-if="storeConfig.merchantId" class="mt-1">
                  <p class="text-xs text-green-600">✓ Merchant ID: {{ storeConfig.merchantId }}</p>
                  <p class="text-xs text-green-600">✓ Kategori: {{ getCategoryName(storeConfig.category) }}</p>
                  <p v-if="qrisTemplate.staticQRISData" class="text-xs text-blue-600">✓ Template QRIS siap untuk generate dinamis</p>
                </div>
              </div>
            </div>
            <div class="flex space-x-2">
              <button
                @click="viewTemplate"
                class="text-blue-600 hover:text-blue-800 text-sm"
              >
                Lihat
              </button>
              <button
                v-if="!storeConfig.merchantId"
                @click="extractMerchantInfoFromTemplate"
                class="text-green-600 hover:text-green-800 text-sm"
              >
                Extract Merchant ID
              </button>
              <button
                @click="removeTemplate"
                class="text-red-600 hover:text-red-800 text-sm"
              >
                Hapus
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Generate QR Dinamis -->
    <div class="bg-white rounded-lg shadow mb-8">
      <div class="px-6 py-4 border-b border-gray-200">
        <h2 class="text-xl font-semibold text-gray-900">Generate QR Dinamis</h2>
        <p class="text-gray-600 text-sm mt-1">Buat QR dengan nominal yang dapat disesuaikan</p>
      </div>
      
      <div class="p-6">
        <div v-if="!hasQRISTemplate" class="text-center py-8">
          <svg class="mx-auto h-12 w-12 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-2.5L13.732 4c-.77-.833-1.964-.833-2.732 0L3.732 16.5c-.77.833.192 2.5 1.732 2.5z"></path>
          </svg>
          <p class="mt-2 text-sm text-gray-500">Upload QRIS template terlebih dahulu untuk dapat generate QR dinamis</p>
        </div>

        <div v-else class="space-y-4">
          <div class="bg-blue-50 border border-blue-200 rounded-lg p-4">
            <div class="flex">
              <div class="flex-shrink-0">
                <svg class="h-5 w-5 text-blue-400" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd" />
                </svg>
              </div>
              <div class="ml-3">
                <h3 class="text-sm font-medium text-blue-800">QRIS Dinamis</h3>
                <div class="mt-2 text-sm text-blue-700">
                  <p>QR yang di-generate menggunakan format QRIS standar yang dapat di-scan oleh aplikasi e-wallet seperti:</p>
                  <ul class="list-disc list-inside mt-1">
                    <li>DANA, OVO, GoPay, LinkAja</li>
                    <li>Aplikasi mobile banking</li>
                    <li>QRIS Scanner lainnya</li>
                  </ul>
                  <div v-if="storeConfig.merchantId" class="mt-2 p-2 bg-green-100 rounded">
                    <p class="text-xs text-green-800">
                      ✓ Menggunakan Merchant ID dari template: <strong>{{ storeConfig.merchantId }}</strong>
                    </p>
                    <p class="text-xs text-green-700 mt-1">
                      QR menggunakan format EMV standar QRIS Indonesia dengan CRC16 checksum
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-2">Nominal (Rp)</label>
              <input
                v-model="qrForm.amount"
                type="number"
                placeholder="Masukkan nominal"
                class="w-full border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500"
              />
            </div>
            
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-2">Keterangan (Opsional)</label>
              <input
                v-model="qrForm.description"
                type="text"
                placeholder="Contoh: Pembayaran makanan"
                class="w-full border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500"
              />
            </div>
          </div>

          <div class="flex justify-end">
            <button
              @click="generateQR"
              :disabled="!qrForm.amount || !storeConfig.merchantId"
              class="bg-green-600 hover:bg-green-700 disabled:bg-gray-400 text-white px-6 py-2 rounded-md text-sm font-medium"
            >
              Generate QRIS
            </button>
          </div>
          
          <div v-if="!storeConfig.merchantId" class="mt-2 text-center">
            <p class="text-xs text-red-600">⚠️ Upload template QRIS terlebih dahulu untuk dapat generate QR</p>
          </div>
        </div>
      </div>
    </div>

    <!-- QR History -->
    <div class="bg-white rounded-lg shadow">
      <div class="px-6 py-4 border-b border-gray-200">
        <h2 class="text-xl font-semibold text-gray-900">History QR Generated</h2>
      </div>
      
      <div class="overflow-x-auto">
        <table class="min-w-full divide-y divide-gray-200">
          <thead class="bg-gray-50">
            <tr>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Tanggal</th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Nominal</th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Keterangan</th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
            </tr>
          </thead>
          <tbody class="bg-white divide-y divide-gray-200">
            <tr v-for="qr in qrHistory" :key="qr.id">
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ formatDate(qr.createdAt) }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">Rp {{ formatNumber(qr.amount) }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ qr.description || '-' }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                <button
                  @click="viewQR(qr)"
                  class="text-blue-600 hover:text-blue-900 mr-3"
                >
                  Lihat QR
                </button>
                <button
                  @click="downloadQR(qr)"
                  class="text-green-600 hover:text-green-900"
                >
                  Download
                </button>
              </td>
            </tr>
          </tbody>
        </table>
        
        <div v-if="qrHistory.length === 0" class="text-center py-8">
          <p class="text-gray-500">Belum ada QR yang di-generate</p>
        </div>
      </div>
    </div>

    <!-- QR Preview Modal -->
    <div v-if="showQRModal" class="fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full z-50">
      <div class="relative top-20 mx-auto p-5 border w-96 shadow-lg rounded-md bg-white">
        <div class="mt-3">
          <div class="flex justify-between items-center mb-4">
            <h3 class="text-lg font-medium text-gray-900">Preview QR</h3>
            <button @click="showQRModal = false" class="text-gray-400 hover:text-gray-600">
              ✕
            </button>
          </div>
          
          <div class="text-center">
            <div v-if="selectedQR" class="mb-4">
              <canvas ref="qrCanvas" class="mx-auto border"></canvas>
              <div class="mt-4 p-3 bg-gray-50 rounded-lg text-left">
                <h4 class="font-semibold text-gray-900 mb-2">Detail QRIS:</h4>
                <p class="text-sm text-gray-600"><strong>Nominal:</strong> Rp {{ formatNumber(selectedQR.amount) }}</p>
                <p v-if="selectedQR.description" class="text-sm text-gray-600"><strong>Keterangan:</strong> {{ selectedQR.description }}</p>
                <p class="text-sm text-gray-600"><strong>Merchant ID:</strong> {{ storeConfig.merchantId }}</p>
                <p class="text-sm text-gray-600"><strong>Kategori:</strong> {{ getCategoryName(storeConfig.category) }}</p>
                <p class="text-sm text-gray-600"><strong>Tanggal:</strong> {{ formatDate(selectedQR.createdAt) }}</p>
                <div class="mt-3 p-2 bg-blue-50 rounded">
                  <p class="text-xs text-blue-800"><strong>QRIS Data:</strong></p>
                  <p class="text-xs text-blue-600 font-mono break-all">{{ selectedQR.qrData.substring(0, 100) }}...</p>
                  <p class="text-xs text-blue-600 mt-1">Length: {{ selectedQR.qrData.length }} chars</p>
                </div>
              </div>
            </div>
            
            <div class="flex justify-center space-x-3 mt-4">
              <button
                @click="downloadCurrentQR"
                class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-md text-sm font-medium"
              >
                Download
              </button>
              <button
                @click="showQRModal = false"
                class="bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded-md text-sm font-medium"
              >
                Tutup
              </button>
            </div>
          </div>
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
const qrisTemplate = ref(null)
const qrHistory = ref([])
const qrForm = ref({
  amount: '',
  description: ''
})
const showQRModal = ref(false)
const selectedQR = ref(null)
const fileInput = ref(null)
const qrCanvas = ref(null)
const storeConfig = ref({
  merchantId: '',
  category: '5812'
})

// Computed
const hasQRISTemplate = computed(() => qrisTemplate.value !== null)
const lastGenerated = computed(() => {
  if (qrHistory.value.length === 0) return null
  const last = qrHistory.value[qrHistory.value.length - 1]
  return formatDate(last.createdAt)
})

// Methods
const loadData = () => {
  const userId = getCurrentUser().id
  console.log('Loading data for user:', userId)
  
  // Load QRIS template
  const template = localStorage.getItem(`qrisTemplate_${userId}`)
  if (template) {
    qrisTemplate.value = JSON.parse(template)
    console.log('QRIS template loaded:', qrisTemplate.value.name)
    
    // Auto extract merchant info if template exists but no merchant ID
    const config = localStorage.getItem(`storeConfig_${userId}`)
    if (config) {
      storeConfig.value = JSON.parse(config)
      console.log('Store config loaded:', storeConfig.value)
    }
    
    if (!storeConfig.value.merchantId) {
      console.log('No merchant ID found, attempting auto extraction...')
      extractMerchantInfoFromTemplate()
    }
  }

  // Load QR history
  const history = localStorage.getItem(`qrHistory_${userId}`)
  if (history) {
    qrHistory.value = JSON.parse(history)
    console.log('QR history loaded:', qrHistory.value.length, 'items')
  }

  // Load store config (if not loaded above)
  if (!storeConfig.value.merchantId) {
    const config = localStorage.getItem(`storeConfig_${userId}`)
    if (config) {
      storeConfig.value = JSON.parse(config)
      console.log('Store config loaded (fallback):', storeConfig.value)
    }
  }
}

const saveStoreConfig = () => {
  const userId = getCurrentUser().id
  console.log('Saving store config for user:', userId, storeConfig.value)
  localStorage.setItem(`storeConfig_${userId}`, JSON.stringify(storeConfig.value))
  showNotification('Konfigurasi store berhasil disimpan', 'success')
}

const getCurrentUser = () => {
  const user = localStorage.getItem('currentUser')
  return user ? JSON.parse(user) : null
}

const handleFileUpload = (event) => {
  const file = event.target.files[0]
  if (!file) return

  console.log('File selected:', file.name, file.type, file.size)
  
  const reader = new FileReader()
  reader.onload = (e) => {
    qrisTemplate.value = {
      name: file.name,
      data: e.target.result,
      uploadedAt: new Date().toISOString()
    }
    
    console.log('Template uploaded:', qrisTemplate.value.name)
    
    // Extract Merchant ID from QRIS template
    extractMerchantInfoFromTemplate()
    
    localStorage.setItem(`qrisTemplate_${getCurrentUser().id}`, JSON.stringify(qrisTemplate.value))
    showNotification('QRIS template berhasil diupload', 'success')
  }
  reader.readAsDataURL(file)
}

const extractMerchantInfoFromTemplate = async () => {
  try {
    console.log('Starting extraction from template:', qrisTemplate.value.name)
    
    // Jika file adalah gambar QR, kita perlu decode QR code-nya
    if (qrisTemplate.value.data.startsWith('data:image/')) {
      console.log('Processing image file...')
      
      // Decode QR code dari gambar ASLI
      const qrisData = await decodeQRFromImage(qrisTemplate.value.data)
      if (qrisData) {
        console.log('QR decoded from image:', qrisData)
        
        // Simpan data QRIS statis untuk digunakan sebagai template
        qrisTemplate.value.staticQRISData = qrisData
        localStorage.setItem(`qrisTemplate_${getCurrentUser().id}`, JSON.stringify(qrisTemplate.value))
        
        const merchantInfo = parseQRISData(qrisData)
        if (merchantInfo) {
          storeConfig.value.merchantId = merchantInfo.merchantId
          storeConfig.value.category = merchantInfo.category
          saveStoreConfig()
          showNotification('Merchant ID berhasil diekstrak dari template QRIS asli', 'success')
          console.log('Merchant info from real QRIS:', merchantInfo)
          return
        }
      }
      
      // Jika decode gagal, berikan pesan error
      console.log('Failed to decode QR from image')
      showNotification('Gagal membaca QR code dari gambar. Pastikan gambar jelas dan berformat QRIS.', 'error')
      
    } else if (qrisTemplate.value.data.startsWith('data:text/')) {
      console.log('Processing text file...')
      // Jika file adalah text/QRIS data
      const qrisData = atob(qrisTemplate.value.data.split(',')[1])
      console.log('QRIS data from text:', qrisData)
      
      // Simpan data QRIS statis untuk digunakan sebagai template
      qrisTemplate.value.staticQRISData = qrisData
      localStorage.setItem(`qrisTemplate_${getCurrentUser().id}`, JSON.stringify(qrisTemplate.value))
      
      const merchantInfo = parseQRISData(qrisData)
      if (merchantInfo) {
        storeConfig.value.merchantId = merchantInfo.merchantId
        storeConfig.value.category = merchantInfo.category
        saveStoreConfig()
        showNotification('Merchant ID berhasil diekstrak dari template', 'success')
        console.log('Merchant info from text:', merchantInfo)
      } else {
        showNotification('Format QRIS tidak valid', 'error')
      }
    }
  } catch (error) {
    console.error('Error extracting merchant info:', error)
    showNotification('Gagal mengekstrak Merchant ID dari template', 'error')
  }
}

const decodeQRFromImage = async (imageData) => {
  try {
    console.log('Attempting to decode QR from image...')
    
    // Try with jsQR library first
    try {
      const jsQR = await import('jsqr')
      
      // Create canvas element to process image
      const canvas = document.createElement('canvas')
      const ctx = canvas.getContext('2d')
      const img = new Image()
      
      return new Promise((resolve) => {
        img.onload = () => {
          canvas.width = img.width
          canvas.height = img.height
          ctx.drawImage(img, 0, 0)
          
          // Get image data
          const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height)
          
          // Decode QR code with jsQR
          const code = jsQR.default(imageData.data, imageData.width, imageData.height)
          
          if (code) {
            console.log('QR decoded successfully with jsQR:', code.data)
            resolve(code.data)
          } else {
            console.log('jsQR failed to decode, trying fallback...')
            resolve(null)
          }
        }
        
        img.onerror = () => {
          console.log('Image load error')
          resolve(null)
        }
        
        img.src = imageData
      })
    } catch (jsqrError) {
      console.log('jsQR import failed, trying qrcode-reader...', jsqrError)
      
      // Fallback to qrcode-reader
      try {
        const QrCodeReader = await import('qrcode-reader')
        
        const canvas = document.createElement('canvas')
        const ctx = canvas.getContext('2d')
        const img = new Image()
        
        return new Promise((resolve) => {
          img.onload = () => {
            canvas.width = img.width
            canvas.height = img.height
            ctx.drawImage(img, 0, 0)
            
            const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height)
            
            const qr = new QrCodeReader.default()
            qr.callback = (err, value) => {
              if (err) {
                console.error('QR decode error with qrcode-reader:', err)
                resolve(null)
              } else {
                console.log('QR decoded successfully with qrcode-reader:', value.result)
                resolve(value.result)
              }
            }
            qr.decode(imageData)
          }
          
          img.onerror = () => {
            resolve(null)
          }
          
          img.src = imageData
        })
      } catch (qrReaderError) {
        console.error('Both QR libraries failed:', qrReaderError)
        return null
      }
    }
  } catch (error) {
    console.error('Error in decodeQRFromImage:', error)
    return null
  }
}

const parseQRISData = (qrisString) => {
  try {
    console.log('Parsing QRIS data:', qrisString)
    
    if (!qrisString || typeof qrisString !== 'string') {
      console.error('Invalid QRIS string')
      return null
    }
    
    const merchantInfo = {}
    let position = 0
    
    // Parse EMV format: [TAG][LENGTH][VALUE]
    while (position < qrisString.length - 4) {
      const tag = qrisString.substr(position, 2)
      const lengthStr = qrisString.substr(position + 2, 2)
      const length = parseInt(lengthStr, 10)
      
      if (isNaN(length) || length <= 0) {
        position += 4
        continue
      }
      
      const value = qrisString.substr(position + 4, length)
      position += 4 + length
      
      console.log(`Tag: ${tag}, Length: ${length}, Value: ${value}`)
      
      switch (tag) {
        case '26': // Merchant Account Information
          console.log('Found Merchant Account Information:', value)
          // Parse nested data in tag 26
          const merchantData = parseNestedEMV(value)
          if (merchantData['01']) {
            merchantInfo.merchantId = merchantData['01']
            console.log('Extracted Merchant ID:', merchantInfo.merchantId)
          }
          if (merchantData['02']) {
            merchantInfo.merchantName = merchantData['02']
            console.log('Extracted Merchant Name:', merchantInfo.merchantName)
          }
          break
          
        case '52': // Merchant Category Code
          merchantInfo.category = value
          console.log('Extracted Category:', merchantInfo.category)
          break
          
        case '53': // Transaction Currency
          merchantInfo.currency = value
          console.log('Extracted Currency:', merchantInfo.currency)
          break
          
        case '54': // Transaction Amount
          merchantInfo.amount = value
          console.log('Extracted Amount:', merchantInfo.amount)
          break
          
        case '58': // Country Code
          merchantInfo.countryCode = value
          console.log('Extracted Country Code:', merchantInfo.countryCode)
          break
          
        case '59': // Merchant Name
          merchantInfo.merchantName = value
          console.log('Extracted Merchant Name (59):', merchantInfo.merchantName)
          break
          
        case '60': // Merchant City
          merchantInfo.merchantCity = value
          console.log('Extracted Merchant City:', merchantInfo.merchantCity)
          break
      }
    }
    
    console.log('Final parsed merchant info:', merchantInfo)
    return merchantInfo.merchantId ? merchantInfo : null
    
  } catch (error) {
    console.error('Error parsing QRIS data:', error)
    return null
  }
}

const parseNestedEMV = (data) => {
  const result = {}
  let position = 0
  
  while (position < data.length - 4) {
    const tag = data.substr(position, 2)
    const lengthStr = data.substr(position + 2, 2)
    const length = parseInt(lengthStr, 10)
    
    if (isNaN(length) || length <= 0) {
      position += 4
      continue
    }
    
    const value = data.substr(position + 4, length)
    result[tag] = value
    position += 4 + length
  }
  
  return result
}

const removeTemplate = () => {
  if (confirm('Apakah Anda yakin ingin menghapus template QRIS?')) {
    qrisTemplate.value = null
    localStorage.removeItem(`qrisTemplate_${getCurrentUser().id}`)
    showNotification('Template QRIS berhasil dihapus', 'success')
  }
}

const viewTemplate = () => {
  if (qrisTemplate.value) {
    window.open(qrisTemplate.value.data, '_blank')
  }
}

const generateQR = () => {
  if (!qrForm.value.amount) {
    showNotification('Masukkan nominal terlebih dahulu', 'error')
    return
  }

  if (!storeConfig.value.merchantId) {
    showNotification('Upload template QRIS terlebih dahulu untuk mendapatkan Merchant ID', 'error')
    return
  }

  // Generate QRIS format yang bisa di-scan
  const qrisData = generateQRISData(parseInt(qrForm.value.amount), qrForm.value.description)

  const qrData = {
    id: Date.now(),
    amount: parseInt(qrForm.value.amount),
    description: qrForm.value.description,
    createdAt: new Date().toISOString(),
    qrData: qrisData
  }

  qrHistory.value.push(qrData)
  localStorage.setItem(`qrHistory_${getCurrentUser().id}`, JSON.stringify(qrHistory.value))

  // Update store stats
  updateStoreStats()

  showNotification('QR berhasil di-generate', 'success')
  
  // Reset form
  qrForm.value = {
    amount: '',
    description: ''
  }
}

const generateQRISData = (amount, description) => {
  console.log('Generating QRIS with amount:', amount, 'description:', description)
  
  // Gunakan template QRIS statis sebagai base
  if (!qrisTemplate.value || !qrisTemplate.value.staticQRISData) {
    console.error('No static QRIS template found')
    showNotification('Template QRIS statis diperlukan untuk generate QR dinamis', 'error')
    return null
  }
  
  const staticQRIS = qrisTemplate.value.staticQRISData
  console.log('Using static QRIS as base:', staticQRIS.substring(0, 50) + '...')
  
  // Modifikasi QRIS statis menjadi dinamis
  const nominalStr = amount.toString()
  
  // Ubah dari static (010211) ke dynamic (010212)
  let qrisModified = staticQRIS.slice(0, -4).replace("010211", "010212")
  
  // Split berdasarkan country code
  let qrisParts = qrisModified.split("5802ID")
  
  // Tambahkan amount
  let amountField = "54" + pad(nominalStr.length) + nominalStr
  
  // Tambahkan fee jika diperlukan (opsional)
  let tax = ""
  
  // Gabungkan bagian-bagian
  amountField += (tax.length === 0) ? "5802ID" : tax + "5802ID"
  let output = qrisParts[0].trim() + amountField + qrisParts[1].trim()
  
  // Hitung dan tambahkan CRC
  output += toCRC16(output)
  
  console.log('Generated dynamic QRIS:', output)
  console.log('QRIS length:', output.length)
  
  return output
}

const pad = (number) => {
  return number < 10 ? '0' + number : number.toString()
}

const toCRC16 = (input) => {
  let crc = 0xFFFF
  for (let i = 0; i < input.length; i++) {
    crc ^= input.charCodeAt(i) << 8
    for (let j = 0; j < 8; j++) {
      crc = (crc & 0x8000) ? (crc << 1) ^ 0x1021 : crc << 1
    }
  }
  
  let hex = (crc & 0xFFFF).toString(16).toUpperCase()
  return hex.length === 3 ? "0" + hex : hex
}

const updateStoreStats = () => {
  const stores = JSON.parse(localStorage.getItem('stores') || '[]')
  const currentUser = getCurrentUser()
  const storeIndex = stores.findIndex(store => store.email === currentUser.email)
  
  if (storeIndex !== -1) {
    stores[storeIndex].qrGenerated = qrHistory.value.length
    localStorage.setItem('stores', JSON.stringify(stores))
  }
}

const viewQR = async (qr) => {
  selectedQR.value = qr
  showQRModal.value = true
  
  // Generate QR code on canvas
  await nextTick()
  if (qrCanvas.value) {
    try {
      const QRCode = await import('qrcode')
      await QRCode.toCanvas(qrCanvas.value, qr.qrData, {
        width: 200,
        margin: 2
      })
    } catch (error) {
      console.error('Error generating QR:', error)
      showNotification('Error generating QR code', 'error')
    }
  }
}

const downloadQR = async (qr) => {
  try {
    const QRCode = await import('qrcode')
    const canvas = document.createElement('canvas')
    await QRCode.toCanvas(canvas, qr.qrData, {
      width: 300,
      margin: 2
    })
    const link = document.createElement('a')
    link.download = `QR_${qr.amount}_${formatDate(qr.createdAt)}.png`
    link.href = canvas.toDataURL()
    link.click()
  } catch (error) {
    console.error('Error generating QR:', error)
    showNotification('Error generating QR code', 'error')
  }
}

const downloadCurrentQR = () => {
  if (selectedQR.value) {
    downloadQR(selectedQR.value)
  }
}

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString('id-ID', {
    year: 'numeric',
    month: 'short',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  })
}

const formatNumber = (number) => {
  return new Intl.NumberFormat('id-ID').format(number)
}

const getCategoryName = (categoryCode) => {
  const categories = {
    '5812': 'Restoran',
    '5411': 'Supermarket',
    '5999': 'Toko Umum',
    '5814': 'Fast Food',
    '5311': 'Department Store',
    '5993': 'Toko Kue'
  }
  return categories[categoryCode] || 'Lainnya'
}

// Check authentication
onMounted(() => {
  const currentUser = localStorage.getItem('currentUser')
  if (!currentUser) {
    navigateTo('/login')
    return
  }
  
  const user = JSON.parse(currentUser)
  if (user.role !== 'store') {
    navigateTo('/login')
    return
  }
  
  loadData()
})
</script>
