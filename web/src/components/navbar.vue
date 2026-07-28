<script setup>
import logo from '@/assets/img/logo.webp'
import { ref, onMounted, onUnmounted } from 'vue'

const currentTime = ref('')

let timer = null

const updateClock = () => {
  currentTime.value = new Date().toLocaleTimeString()
}

onMounted(() => {
  updateClock()
  // Update time every second
  timer = setInterval(updateClock, 1000)
})

onUnmounted(() => {

  clearInterval(timer)
})
</script>

<template>
  <div class="flex h-20 bg-[#1c1917] items-center justify-between ml-4">
    
    <div class="h-full w-30 flex-none flex  justify-centeitems-centerr">
      <img
        :src="logo"
        class="block h-full max-h-full w-full object-contain"
      />
    </div>

    
    

    <div class="flex shrink items-center justify-center p-6 bg-[#1c1917] rounded-2xl shadow-xl h-full max-w-xs mx-auto border ">
      <div class="text-center">
      
        <!-- Live Clock Display -->
        <div class="mt-1 text-3xl font-mono font-bold text-white tracking-wider">
          {{ currentTime || '00:00:00' }}
        </div>
      </div>
    </div>
    
    <div class="h-full w-30 flex items-center justify-center flex-none mr-4">
      <p class="font-mono text-2xl font-bold tracking-wide text-white">
        al adhan
      </p>
    </div>

  </div>
</template>