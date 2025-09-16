<template>
  <div class="fixed top-4 right-4 z-50">
    <div 
      :class="[
        'px-6 py-4 rounded-lg shadow-lg max-w-sm',
        type === 'success' ? 'bg-green-500 text-white' : 'bg-red-500 text-white'
      ]"
    >
      <div class="flex items-center justify-between">
        <span>{{ message }}</span>
        <button 
          @click="$emit('close')"
          class="ml-4 text-white hover:text-gray-200"
        >
          ✕
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  message: {
    type: String,
    required: true
  },
  type: {
    type: String,
    default: 'success'
  }
})

defineEmits(['close'])
</script>
