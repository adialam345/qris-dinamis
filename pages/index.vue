<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
    <div class="container mx-auto px-4 py-4">
      <!-- Header -->
      <div class="text-center mb-6">
        <div class="inline-flex items-center justify-center w-12 h-12 md:w-16 md:h-16 bg-gradient-to-r from-blue-500 to-indigo-600 rounded-full mb-3">
          <svg class="w-6 h-6 md:w-8 md:h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v1m6 11h2m-6 0h-2v4m0-11v3m0 0h.01M12 12h4.01M16 20h4M4 12h4m12 0h.01M5 8h2a1 1 0 001-1V5a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1zm12 0h2a1 1 0 001-1V5a1 1 0 00-1-1h-2a1 1 0 00-1 1v2a1 1 0 001 1zM5 20h2a1 1 0 001-1v-2a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1z"></path>
          </svg>
        </div>
        <h1 class="text-2xl md:text-3xl font-bold text-gray-900 mb-1">
          QRIS Dinamis
        </h1>
        <p class="text-sm md:text-base text-gray-600">
          Upload QRIS → Auto extract → Masukkan nominal
        </p>
      </div>

      <!-- Progress Steps -->
      <div class="flex items-center justify-center space-x-4 md:space-x-8 mb-6">
        <!-- Step 1: Upload -->
        <div class="flex items-center">
          <div :class="[
            'w-8 h-8 md:w-10 md:h-10 rounded-full flex items-center justify-center text-xs md:text-sm font-semibold',
            qrisTemplate ? 'bg-green-500 text-white' : 'bg-gray-200 text-gray-600'
          ]">
            <svg v-if="qrisTemplate" class="w-4 h-4 md:w-5 md:h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
            </svg>
            <span v-else>1</span>
          </div>
          </div>
          
        <!-- Arrow -->
        <div class="w-6 md:w-8 h-0.5 bg-gray-300"></div>

        <!-- Step 2: Extract -->
        <div class="flex items-center">
          <div :class="[
            'w-8 h-8 md:w-10 md:h-10 rounded-full flex items-center justify-center text-xs md:text-sm font-semibold',
            merchantInfo ? 'bg-green-500 text-white' : (qrisTemplate ? 'bg-blue-500 text-white' : 'bg-gray-200 text-gray-600')
          ]">
            <svg v-if="merchantInfo" class="w-4 h-4 md:w-5 md:h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
              </svg>
            <svg v-else-if="qrisTemplate && isExtracting" class="w-4 h-4 md:w-5 md:h-5 animate-spin" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
            </svg>
            <span v-else>2</span>
          </div>
        </div>

        <!-- Arrow -->
        <div class="w-6 md:w-8 h-0.5 bg-gray-300"></div>

        <!-- Step 3: Generate -->
        <div class="flex items-center">
          <div :class="[
            'w-8 h-8 md:w-10 md:h-10 rounded-full flex items-center justify-center text-xs md:text-sm font-semibold',
            merchantInfo ? 'bg-blue-500 text-white' : 'bg-gray-200 text-gray-600'
          ]">
            <span>3</span>
          </div>
        </div>
      </div>

      <!-- Main Content -->
      <div class="max-w-md md:max-w-lg lg:max-w-2xl mx-auto">
        <!-- Upload Section -->
        <div v-if="!qrisTemplate" class="bg-white rounded-xl shadow-lg p-6 md:p-8 mb-4">
          <div class="text-center">
            <div class="mx-auto w-12 h-12 md:w-16 md:h-16 bg-blue-100 rounded-full flex items-center justify-center mb-4">
              <svg class="w-6 h-6 md:w-8 md:h-8 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path>
              </svg>
            </div>
            <h3 class="text-lg md:text-xl font-semibold text-gray-900 mb-2">Upload QRIS Statis</h3>
            <p class="text-sm md:text-base text-gray-600 mb-4">PNG, JPG atau file data QR</p>
                <input
                  ref="fileInput"
                  type="file"
                  accept="image/*,.txt"
                  @change="handleFileUpload"
                  class="sr-only"
                />
              <button
                @click="$refs.fileInput.click()"
              class="bg-blue-500 hover:bg-blue-600 text-white px-6 md:px-8 py-3 md:py-4 rounded-lg text-sm md:text-base font-medium transition-colors"
              >
                Pilih File
              </button>
          </div>
            </div>

        <!-- Template Info -->
        <div v-else class="bg-white rounded-xl shadow-lg p-4 md:p-6 mb-4">
          <div class="flex items-center justify-between">
                <div class="flex items-center">
              <div class="w-10 h-10 md:w-12 md:h-12 bg-green-500 rounded-full flex items-center justify-center mr-3">
                <svg class="w-5 h-5 md:w-6 md:h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                  </svg>
              </div>
                  <div>
                <p class="font-medium text-gray-900 text-sm md:text-base">{{ qrisTemplate.name }}</p>
                <div v-if="merchantInfo" class="text-xs md:text-sm text-green-600">
                  ✓ {{ getCategoryName(merchantInfo.category) }}
                    </div>
                <div v-else-if="isExtracting" class="text-xs md:text-sm text-blue-600">
                  <svg class="w-3 h-3 md:w-4 md:h-4 inline mr-1 animate-spin" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
                  </svg>
                  Extracting...
                  </div>
                </div>
            </div>
                  <button
                    @click="removeTemplate"
              class="text-red-500 hover:text-red-700 text-sm md:text-base"
                  >
                    Hapus
                  </button>
          </div>
        </div>

        <!-- Generate Form -->
        <div v-if="qrisTemplate && merchantInfo" class="bg-white rounded-xl shadow-lg p-4 md:p-6 mb-4">
          <h3 class="text-lg md:text-xl font-semibold text-gray-900 mb-4">Masukkan Nominal</h3>
          
          <!-- Amount Input -->
          <div class="mb-4">
            <div class="relative">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <span class="text-gray-500 font-medium">Rp</span>
              </div>
              <input
                v-model="qrForm.amount"
                type="text"
                placeholder="0"
                class="w-full pl-10 pr-4 py-3 md:py-4 text-lg md:text-xl border-2 border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition-all"
                @input="formatAmount"
              />
            </div>

            <!-- Quick Amount Buttons -->
            <div class="mt-3">
              <div class="grid grid-cols-3 md:grid-cols-6 gap-2">
                <button
                  v-for="amount in [25000, 50000, 100000, 200000, 500000, 1000000]"
                  :key="amount"
                  @click="setQuickAmount(amount)"
                  class="bg-gray-100 hover:bg-gray-200 text-gray-700 px-2 py-2 rounded text-xs md:text-sm font-medium transition-colors"
                >
                  {{ formatNumber(amount) }}
                </button>
                  </div>
                </div>
              </div>

          <!-- Description Input -->
          <div class="mb-4">
                  <input
                    v-model="qrForm.description"
                    type="text"
              placeholder="Keterangan (opsional)"
              class="w-full px-3 py-2 md:py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition-all text-sm md:text-base"
                  />
              </div>

          <!-- Generate Button -->
                <button
                  @click="generateQR"
            :disabled="!qrForm.amount || isGenerating"
            class="w-full bg-blue-500 hover:bg-blue-600 disabled:bg-gray-400 text-white py-3 md:py-4 rounded-lg font-medium transition-colors flex items-center justify-center text-sm md:text-base"
                >
            <svg v-if="isGenerating" class="w-4 h-4 md:w-5 md:h-5 mr-2 animate-spin" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
            </svg>
            {{ isGenerating ? 'Generating...' : 'Generate QRIS' }}
                </button>
              </div>
              
        <!-- Waiting State -->
        <div v-else-if="qrisTemplate && !merchantInfo" class="bg-white rounded-xl shadow-lg p-6 md:p-8 mb-4 text-center">
          <div class="mx-auto w-12 h-12 md:w-16 md:h-16 bg-blue-100 rounded-full flex items-center justify-center mb-3">
            <svg class="w-6 h-6 md:w-8 md:h-8 text-blue-500 animate-spin" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
            </svg>
              </div>
          <p class="text-gray-600 text-sm md:text-base">Mengekstrak data QRIS...</p>
        </div>

        <!-- QR History -->
        <div v-if="qrHistory.length > 0" class="bg-white rounded-xl shadow-lg p-4 md:p-6">
          <div class="flex justify-between items-center mb-3">
            <h3 class="text-lg md:text-xl font-semibold text-gray-900">History ({{ qrHistory.length }})</h3>
            <button
              @click="clearHistory"
              class="text-red-500 hover:text-red-700 text-sm md:text-base"
            >
              Clear All
            </button>
          </div>
          
          <div class="space-y-2 max-h-40 md:max-h-60 overflow-y-auto">
            <div v-for="qr in qrHistory.slice().reverse().slice(0, 3)" :key="qr.id" class="flex items-center justify-between p-3 md:p-4 bg-gray-50 rounded-lg">
              <div class="flex items-center">
                <div class="w-8 h-8 md:w-10 md:h-10 bg-green-500 rounded-full flex items-center justify-center mr-3">
                  <svg class="w-4 h-4 md:w-5 md:h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v1m6 11h2m-6 0h-2v4m0-11v3m0 0h.01M12 12h4.01M16 20h4M4 12h4m12 0h.01M5 8h2a1 1 0 001-1V5a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1zm12 0h2a1 1 0 001-1V5a1 1 0 00-1-1h-2a1 1 0 00-1 1v2a1 1 0 001 1zM5 20h2a1 1 0 001-1v-2a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1z"></path>
                  </svg>
                </div>
                <div>
                  <p class="font-medium text-gray-900 text-sm md:text-base">Rp {{ formatNumber(qr.amount) }}</p>
                  <p class="text-xs md:text-sm text-gray-500">{{ formatDate(qr.createdAt) }}</p>
                </div>
              </div>
              <div class="flex space-x-1 md:space-x-2">
                    <button
                      @click="viewQR(qr)"
                  class="bg-blue-500 hover:bg-blue-600 text-white text-xs md:text-sm px-2 md:px-3 py-1 md:py-2 rounded transition-colors"
                    >
                  Lihat
                    </button>
                    <button
                      @click="downloadQR(qr)"
                  class="bg-green-500 hover:bg-green-600 text-white text-xs md:text-sm px-2 md:px-3 py-1 md:py-2 rounded transition-colors"
                    >
                      Download
                    </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- QR Preview Modal -->
    <div v-if="showQRModal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center p-4 animate-fadeIn">
      <div class="bg-white rounded-xl shadow-xl max-w-sm md:max-w-md w-full animate-slideUp">
        <div class="flex justify-between items-center p-4 md:p-6 border-b">
          <h3 class="text-lg md:text-xl font-semibold text-gray-900">Preview QRIS</h3>
            <button @click="showQRModal = false" class="text-gray-400 hover:text-gray-600">
            <svg class="w-5 h-5 md:w-6 md:h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
            </svg>
            </button>
          </div>
          
        <div class="p-4 md:p-6">
          <div v-if="selectedQR" class="text-center">
            <div class="bg-white border border-gray-200 rounded-lg p-3 md:p-4 mb-4 inline-block">
              <canvas ref="qrCanvas" class="mx-auto"></canvas>
                </div>
            
            <div class="bg-gray-50 rounded-lg p-3 md:p-4 text-left mb-4">
              <div class="space-y-1 text-sm md:text-base">
                <div class="flex justify-between">
                  <span class="text-gray-600">Nominal:</span>
                  <span class="font-semibold text-green-600">Rp {{ formatNumber(selectedQR.amount) }}</span>
                </div>
                <div v-if="selectedQR.description" class="flex justify-between">
                  <span class="text-gray-600">Keterangan:</span>
                  <span class="font-medium text-gray-900">{{ selectedQR.description }}</span>
                </div>
                <div class="flex justify-between">
                  <span class="text-gray-600">Tanggal:</span>
                  <span class="font-medium text-gray-900">{{ formatDate(selectedQR.createdAt) }}</span>
                </div>
              </div>
            </div>
            
            <div class="flex space-x-2 md:space-x-3">
              <button
                @click="downloadCurrentQR"
                class="flex-1 bg-blue-500 hover:bg-blue-600 text-white px-3 md:px-4 py-2 md:py-3 rounded-lg text-sm md:text-base font-medium transition-colors"
              >
                Download
              </button>
              <button
                @click="showQRModal = false"
                class="flex-1 bg-gray-500 hover:bg-gray-600 text-white px-3 md:px-4 py-2 md:py-3 rounded-lg text-sm md:text-base font-medium transition-colors"
              >
                Tutup
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Notification -->
    <div v-if="notification.show" class="fixed top-4 right-4 z-50">
      <div 
        :class="[
          'px-4 py-3 rounded-lg shadow-lg max-w-sm',
          notification.type === 'success' ? 'bg-green-500 text-white' : 'bg-red-500 text-white'
        ]"
      >
        <div class="flex items-center justify-between">
          <span class="text-sm font-medium">{{ notification.message }}</span>
          <button 
            @click="notification.show = false"
            class="ml-3 text-white hover:text-gray-200"
          >
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
            </svg>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'

// State
const qrisTemplate = ref(null)
const merchantInfo = ref(null)
const qrHistory = ref([])
const qrForm = ref({
  amount: '',
  description: ''
})
const showQRModal = ref(false)
const selectedQR = ref(null)
const fileInput = ref(null)
const qrCanvas = ref(null)
const isExtracting = ref(false)
const isGenerating = ref(false)
const notification = ref({
  show: false,
  message: '',
  type: 'success'
})

// Methods
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

const handleFileUpload = async (event) => {
  const file = event.target.files[0]
  if (!file) return

  console.log('File selected:', file.name, file.type, file.size)
  
  const reader = new FileReader()
  reader.onload = async (e) => {
    qrisTemplate.value = {
      name: file.name,
      data: e.target.result,
      uploadedAt: new Date().toISOString()
    }
    
    console.log('Template uploaded:', qrisTemplate.value.name)
    showNotification('QRIS template berhasil diupload, sedang mengekstrak data...', 'success')
    
    // Auto extract data after upload
    await extractMerchantInfo()
  }
  reader.readAsDataURL(file)
}

const extractMerchantInfo = async () => {
  if (!qrisTemplate.value) return
  
  try {
    isExtracting.value = true
    console.log('Starting extraction from template:', qrisTemplate.value.name)
    
    if (qrisTemplate.value.data.startsWith('data:image/')) {
      console.log('Processing image file...')
      
      const qrisData = await decodeQRFromImage(qrisTemplate.value.data)
      if (qrisData) {
        console.log('QR decoded from image:', qrisData)
        
        qrisTemplate.value.staticQRISData = qrisData
        localStorage.setItem('qrisTemplate', JSON.stringify(qrisTemplate.value))
        
        const info = parseQRISData(qrisData)
        if (info) {
          merchantInfo.value = info
          showNotification('Data berhasil diekstrak! Siap untuk generate QR dinamis', 'success')
          console.log('Merchant info from real QRIS:', info)
          return
        }
      }
      
      console.log('Failed to decode QR from image')
      showNotification('Gagal membaca QR code dari gambar. Pastikan gambar jelas dan berformat QRIS.', 'error')
      
    } else if (qrisTemplate.value.data.startsWith('data:text/')) {
      console.log('Processing text file...')
      const qrisData = atob(qrisTemplate.value.data.split(',')[1])
      console.log('QRIS data from text:', qrisData)
      
      qrisTemplate.value.staticQRISData = qrisData
      localStorage.setItem('qrisTemplate', JSON.stringify(qrisTemplate.value))
      
      const info = parseQRISData(qrisData)
      if (info) {
        merchantInfo.value = info
        showNotification('Data berhasil diekstrak! Siap untuk generate QR dinamis', 'success')
        console.log('Merchant info from text:', info)
      } else {
        showNotification('Format QRIS tidak valid', 'error')
      }
    }
  } catch (error) {
    console.error('Error extracting merchant info:', error)
    showNotification('Gagal mengekstrak data dari template', 'error')
  } finally {
    isExtracting.value = false
  }
}

const decodeQRFromImage = async (imageData) => {
  try {
    console.log('Attempting to decode QR from image...')
    
    try {
      const jsQR = await import('jsqr')
      
      const canvas = document.createElement('canvas')
      const ctx = canvas.getContext('2d')
      const img = new Image()
      
      return new Promise((resolve) => {
        img.onload = () => {
          canvas.width = img.width
          canvas.height = img.height
          ctx.drawImage(img, 0, 0)
          
          const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height)
          
          const code = jsQR.default(imageData.data, imageData.width, imageData.height)
          
          if (code) {
            console.log('QR decoded successfully with jsQR:', code.data)
            resolve(code.data)
          } else {
            console.log('jsQR failed to decode')
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
      console.log('jsQR import failed:', jsqrError)
      return null
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
        case '26':
          console.log('Found Merchant Account Information:', value)
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
          
        case '52':
          merchantInfo.category = value
          console.log('Extracted Category:', merchantInfo.category)
          break
          
        case '53':
          merchantInfo.currency = value
          console.log('Extracted Currency:', merchantInfo.currency)
          break
          
        case '54':
          merchantInfo.amount = value
          console.log('Extracted Amount:', merchantInfo.amount)
          break
          
        case '58':
          merchantInfo.countryCode = value
          console.log('Extracted Country Code:', merchantInfo.countryCode)
          break
          
        case '59':
          merchantInfo.merchantName = value
          console.log('Extracted Merchant Name (59):', merchantInfo.merchantName)
          break
          
        case '60':
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

const generateQR = async () => {
  if (!qrForm.value.amount) {
    showNotification('Masukkan nominal terlebih dahulu', 'error')
    return
  }

  if (!qrisTemplate.value.staticQRISData) {
    showNotification('Extract data dari template terlebih dahulu', 'error')
    return
  }

  // Parse amount yang sudah diformat
  const amount = parseInt(qrForm.value.amount.replace(/\D/g, ''))
  if (!amount || amount <= 0) {
    showNotification('Nominal tidak valid', 'error')
    return
  }

  isGenerating.value = true

  try {
    const qrisData = generateQRISData(amount, qrForm.value.description)

  if (!qrisData) {
    showNotification('Gagal generate QRIS', 'error')
    return
  }

  const qrData = {
    id: Date.now(),
      amount: amount,
    description: qrForm.value.description,
    createdAt: new Date().toISOString(),
    qrData: qrisData
  }

  qrHistory.value.push(qrData)
  localStorage.setItem('qrHistory', JSON.stringify(qrHistory.value))

    showNotification('QRIS dinamis berhasil dibuat!', 'success')
    
    // Auto show QR modal after generation
    selectedQR.value = qrData
    showQRModal.value = true
    
    // Generate QR canvas after modal is shown
    await nextTick()
    setTimeout(() => {
      if (qrCanvas.value) {
        try {
          import('qrcode').then(QRCode => {
            generateQRWithLogo(qrCanvas.value, qrData.qrData)
          })
        } catch (error) {
          console.error('Error generating QR:', error)
          showNotification('Error generating QR code', 'error')
        }
      }
    }, 100)
  
  qrForm.value = {
    amount: '',
    description: ''
    }
  } finally {
    isGenerating.value = false
  }
}

const generateQRISData = (amount, description) => {
  console.log('Generating QRIS with amount:', amount, 'description:', description)
  
  if (!qrisTemplate.value || !qrisTemplate.value.staticQRISData) {
    console.error('No static QRIS template found')
    return null
  }
  
  const staticQRIS = qrisTemplate.value.staticQRISData
  console.log('Using static QRIS as base:', staticQRIS.substring(0, 50) + '...')
  
  const nominalStr = amount.toString()
  
  let qrisModified = staticQRIS.slice(0, -4).replace("010211", "010212")
  
  let qrisParts = qrisModified.split("5802ID")
  
  let amountField = "54" + pad(nominalStr.length) + nominalStr
  
  let tax = ""
  
  amountField += (tax.length === 0) ? "5802ID" : tax + "5802ID"
  let output = qrisParts[0].trim() + amountField + qrisParts[1].trim()
  
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

const viewTemplate = () => {
  if (qrisTemplate.value) {
    window.open(qrisTemplate.value.data, '_blank')
  }
}

const removeTemplate = () => {
  if (confirm('Apakah Anda yakin ingin menghapus template QRIS?')) {
    qrisTemplate.value = null
    merchantInfo.value = null
    localStorage.removeItem('qrisTemplate')
    showNotification('Template QRIS berhasil dihapus', 'success')
  }
}

const viewQR = async (qr) => {
  selectedQR.value = qr
  showQRModal.value = true
  
  await nextTick()
  if (qrCanvas.value) {
    try {
      generateQRWithLogo(qrCanvas.value, qr.qrData)
    } catch (error) {
      console.error('Error generating QR:', error)
      showNotification('Error generating QR code', 'error')
    }
  }
}

const downloadQR = async (qr) => {
  try {
    const canvas = document.createElement('canvas')
    await generateQRWithLogo(canvas, qr.qrData, 400)
    const link = document.createElement('a')
    link.download = `QRIS_${qr.amount}_${new Date(qr.createdAt).toISOString().split('T')[0]}.png`
    link.href = canvas.toDataURL()
    link.click()
    showNotification('QR berhasil didownload', 'success')
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

const generateQRWithLogo = async (canvas, qrData, size = 200) => {
  try {
    const QRCode = await import('qrcode')
    
    // Generate QR code
    await QRCode.toCanvas(canvas, qrData, {
      width: size,
      margin: 2,
      color: {
        dark: '#000000',
        light: '#FFFFFF'
      }
    })
    
    // Get canvas context
    const ctx = canvas.getContext('2d')
    
    // Calculate center position
    const centerX = canvas.width / 2
    const centerY = canvas.height / 2
    
    // Create white background rectangle for logo - balanced size for scanning and visibility
    const logoWidth = size * 0.35 // 35% of QR size width (slightly wider, still safe)
    const logoHeight = size * 0.15 // 15% of QR size height (keep height same)
    const logoX = centerX - logoWidth / 2
    const logoY = centerY - logoHeight / 2
    
    // Draw white rectangle background with more elegant rounded corners
    ctx.fillStyle = '#FFFFFF'
    const cornerRadius = 8 // More rounded for elegance
    ctx.beginPath()
    ctx.roundRect(logoX, logoY, logoWidth, logoHeight, cornerRadius)
    ctx.fill()
    
    // Draw elegant border with subtle shadow effect
    ctx.strokeStyle = '#D1D5DB'
    ctx.lineWidth = 1.5
    ctx.stroke()
    
    // Add subtle inner shadow for depth
    ctx.strokeStyle = '#F3F4F6'
    ctx.lineWidth = 1
    ctx.beginPath()
    ctx.roundRect(logoX + 1, logoY + 1, logoWidth - 2, logoHeight - 2, cornerRadius - 1)
    ctx.stroke()
    
    // Add "QRISIN" text with elegant styling - optimized for smaller logo
    ctx.fillStyle = '#1F2937'
    ctx.font = `bold ${logoHeight * 0.7}px 'Segoe UI', 'Roboto', 'Helvetica Neue', Arial, sans-serif`
    ctx.textAlign = 'center'
    ctx.textBaseline = 'middle'
    
    // Add subtle text shadow for elegance
    ctx.shadowColor = 'rgba(0, 0, 0, 0.1)'
    ctx.shadowBlur = 1
    ctx.shadowOffsetX = 0.5
    ctx.shadowOffsetY = 0.5
    
    // Always display as single line
    ctx.fillText('QRISIN', centerX, centerY)
    
    // Reset shadow
    ctx.shadowColor = 'transparent'
    ctx.shadowBlur = 0
    ctx.shadowOffsetX = 0
    ctx.shadowOffsetY = 0
    
  } catch (error) {
    console.error('Error generating QR with logo:', error)
    throw error
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

const formatAmount = (event) => {
  // Format input dengan pemisah ribuan
  const value = event.target.value.replace(/\D/g, '')
  if (value) {
    const formatted = new Intl.NumberFormat('id-ID').format(parseInt(value))
    qrForm.value.amount = formatted
  } else {
    qrForm.value.amount = ''
  }
}

const setQuickAmount = (amount) => {
  qrForm.value.amount = new Intl.NumberFormat('id-ID').format(amount)
}

const setQuickDescription = (description) => {
  qrForm.value.description = description
}

const clearForm = () => {
  qrForm.value = {
    amount: '',
    description: ''
  }
}

const clearHistory = () => {
  if (confirm('Apakah Anda yakin ingin menghapus semua history QR?')) {
    qrHistory.value = []
    localStorage.removeItem('qrHistory')
    showNotification('History QR berhasil dihapus', 'success')
  }
}

const getCategoryName = (categoryCode) => {
  const categories = {
    '5812': 'Restoran',
    '5411': 'Supermarket',
    '5999': 'Toko Umum',
    '5814': 'Fast Food',
    '5311': 'Department Store',
    '5993': 'Toko Kue',
    '5813': 'Minuman',
    '5994': 'Toko Elektronik',
    '5995': 'Toko Pakaian',
    '5996': 'Toko Sepatu',
    '5997': 'Toko Kosmetik',
    '5998': 'Toko Buku',
    '7011': 'Hotel',
    '7012': 'Penginapan',
    '7032': 'Olahraga',
    '7033': 'Fitness',
    '7299': 'Jasa Lainnya',
    '7372': 'Software',
    '7379': 'IT Services',
    '7511': 'Transportasi',
    '7512': 'Taksi',
    '7513': 'Ojek',
    '7514': 'Bus',
    '7515': 'Kereta',
    '7516': 'Pesawat',
    '7517': 'Kapal',
    '7518': 'Rental Kendaraan',
    '7519': 'Parkir',
    '7523': 'Parkir',
    '7531': 'Bengkel',
    '7532': 'Service Motor',
    '7533': 'Service Mobil',
    '7534': 'Service AC',
    '7535': 'Service Elektronik',
    '7536': 'Service Komputer',
    '7537': 'Service HP',
    '7538': 'Service Printer',
    '7539': 'Service Lainnya',
    '7542': 'Car Wash',
    '7549': 'Jasa Cuci',
    '7622': 'Service Elektronik',
    '7623': 'Service AC',
    '7629': 'Service Lainnya',
    '7631': 'Service Komputer',
    '7641': 'Service Furniture',
    '7699': 'Service Lainnya',
    '7832': 'Bioskop',
    '7841': 'Video Rental',
    '7911': 'Karaoke',
    '7922': 'Konser',
    '7929': 'Entertainment',
    '7932': 'Billiard',
    '7933': 'Bowling',
    '7941': 'Sports',
    '7991': 'Museum',
    '7992': 'Zoo',
    '7993': 'Aquarium',
    '7994': 'Theme Park',
    '7995': 'Casino',
    '7996': 'Lottery',
    '7997': 'Gaming',
    '7998': 'Recreation',
    '7999': 'Entertainment',
    '8011': 'Dokter',
    '8021': 'Dentist',
    '8031': 'Osteopath',
    '8041': 'Chiropractor',
    '8042': 'Optometrist',
    '8043': 'Podiatrist',
    '8049': 'Health Services',
    '8050': 'Nursing Home',
    '8062': 'Hospital',
    '8071': 'Medical Lab',
    '8082': 'Dentist',
    '8099': 'Health Services',
    '8111': 'Legal Services',
    '8211': 'School',
    '8220': 'University',
    '8241': 'Correspondence School',
    '8244': 'Business School',
    '8249': 'Trade School',
    '8299': 'Educational Services',
    '8351': 'Child Care',
    '8398': 'Charitable Services',
    '8641': 'Civic Organization',
    '8651': 'Political Organization',
    '8661': 'Religious Organization',
    '8675': 'Automobile Association',
    '8699': 'Membership Organization',
    '8734': 'Testing Laboratory',
    '8911': 'Architectural Services',
    '8931': 'Accounting Services',
    '8999': 'Professional Services',
    '9211': 'Court Costs',
    '9222': 'Fines',
    '9223': 'Bail Bonds',
    '9311': 'Tax Payments',
    '9399': 'Government Services',
    '9401': 'Postal Services',
    '9402': 'Government Services',
    '9950': 'Intra-Company Purchases'
  }
  return categories[categoryCode] || `Kategori ${categoryCode}`
}

// Load data on mount
onMounted(() => {
  const template = localStorage.getItem('qrisTemplate')
  if (template) {
    qrisTemplate.value = JSON.parse(template)
    
    // Jika template sudah ada dan ada staticQRISData, extract merchant info
    if (qrisTemplate.value.staticQRISData) {
      const info = parseQRISData(qrisTemplate.value.staticQRISData)
      if (info) {
        merchantInfo.value = info
      }
    }
  }

  const history = localStorage.getItem('qrHistory')
  if (history) {
    qrHistory.value = JSON.parse(history)
  }
})
</script>

<style scoped>
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fadeIn {
  animation: fadeIn 0.2s ease-out;
}

.animate-slideUp {
  animation: slideUp 0.3s ease-out;
}
</style>
