<script setup>
import { ref } from 'vue'

const searchQuery = ref('')

// Define events to communicate with HomePage
const emit = defineEmits(['search-city', 'use-location'])

const handleSearch = () => {
  if (searchQuery.value.trim()) {
    emit('search-city', searchQuery.value.trim())
  }
}

const handleLocationClick = () => {
  emit('use-location')
}
</script>

<template>
  <div class="h-64 flex justify-center m-4">
    <div class="bg-white w-3/4 h-full flex flex-col items-center justify-evenly rounded-lg shadow-md">
      <p class="font-mono text-2xl font-bold">Search a Place</p>

      <input 
        v-model="searchQuery" 
        @keyup.enter="handleSearch"
        class="p-2 h-12 rounded-lg w-4/5 border border-black focus:outline-none focus:ring-2 focus:ring-blue-500" 
        type="text" 
        placeholder="Enter city name (e.g., Algiers, Paris)..."
      />

      <div class="flex space-x-4">
        <button 
          @click="handleSearch"
          class="border-2 rounded-lg bg-blue-600 text-white hover:bg-blue-700 h-10 items-center flex justify-center w-36 font-semibold transition cursor-pointer"
        >
          Search
        </button>
        <button 
          @click="handleLocationClick"
          class="border-2 rounded-lg bg-emerald-600 text-white hover:bg-emerald-700 h-10 items-center flex justify-center w-36 font-semibold transition cursor-pointer"
        >
          My Location
        </button>
      </div>    

      <p class="text-sm text-gray-500">Please allow permission to access your device location.</p>
    </div>
  </div>
</template>