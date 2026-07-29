<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  cityy:String,
  timings: {
    type: Object,
    default: () => null
  },
  hijriDate: {
    type: Object,
    default: () => null
  },
  loading: Boolean,
  error: String
})

const countdown = ref('00:00:00')
const nextPrayerName = ref('')
let timer = null

// Prayer name map to match AlAdhan API keys with your UI labels
const prayerMap = [
  { key: 'Fajr', label: 'al sobh' },
  { key: 'Dhuhr', label: 'al dhohr' },
  { key: 'Asr', label: 'al aser' },
  { key: 'Maghrib', label: 'al maghreb' },
  { key: 'Isha', label: 'al ichai' }
]

const updateCountdown = () => {
  if (!props.timings) return

  const now = new Date()
  let upcomingTime = null
  let upcomingName = ''

  for (const prayer of prayerMap) {
    const timeStr = props.timings[prayer.key] // Format "HH:MM"
    if (!timeStr) continue

    const [hours, minutes] = timeStr.split(':')
    const prayerDate = new Date()
    prayerDate.setHours(parseInt(hours), parseInt(minutes), 0, 0)

    if (prayerDate > now) {
      upcomingTime = prayerDate
      upcomingName = prayer.label
      break
    }
  }

  // If all prayers today have passed, target tomorrow's Fajr
  if (!upcomingTime) {
    const [hours, minutes] = props.timings['Fajr'].split(':')
    upcomingTime = new Date()
    upcomingTime.setDate(upcomingTime.getDate() + 1)
    upcomingTime.setHours(parseInt(hours), parseInt(minutes), 0, 0)
    upcomingName = 'al sobh'
  }

  nextPrayerName.value = upcomingName

  // Compute time difference
  const diff = upcomingTime - now
  const h = Math.floor(diff / (1000 * 60 * 60))
  const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60))
  const s = Math.floor((diff % (1000 * 60)) / 1000)

  countdown.value = `${String(h).padStart(2, '0')}:${String(m).padStart(2, '0')}:${String(s).padStart(2, '0')}`
}

onMounted(() => {
  timer = setInterval(updateCountdown, 1000)
})

onUnmounted(() => {
  clearInterval(timer)
})
</script>

<template>
  <div class="h-auto flex justify-center m-4">
    <div class="bg-white w-3/4 flex flex-col items-center justify-evenly p-5 rounded-lg shadow-md min-h-[24rem]">
      
      <!-- Hijri Date Display -->
      <div v-if="hijriDate" class="text-gray-700 font-mono font-bold mb-2 flex flex-col items-center">
        <div> {{ hijriDate.day }} {{ hijriDate.month.en }} ({{ hijriDate.month.ar }}) {{ hijriDate.year }} AH </div>
        <div> {{ cityy }}</div>
      </div>

      <!-- Loading / Error States -->
      <div v-if="loading" class="font-mono text-gray-500">Fetching prayer timings...</div>
      <div v-else-if="error" class="font-mono text-red-500">{{ error }}</div>

      <template v-else>
        <!-- Countdown Container -->
        <div class="border-2 border-black rounded-lg flex flex-col items-center justify-center p-3 w-auto min-w-[18rem] mb-4">
          <p class="font-mono text-sm text-gray-600 uppercase">
            Time remaining for <span class="font-bold text-black">{{ nextPrayerName || 'next prayer' }}</span>
          </p>
          <p class="font-mono text-3xl font-bold text-emerald-600 mt-1">
            {{ countdown }}
          </p>
        </div>

        <!-- Prayer Timings Bar -->
        <div class="border-2 border-black rounded-lg h-24 w-4/5 flex flex-row overflow-hidden">
          <div 
            v-for="(prayer, index) in prayerMap" 
            :key="prayer.key" 
            :class="['flex flex-col items-center justify-center h-full w-1/5', index < prayerMap.length - 1 ? 'border-r-2 border-black' : '']"
          >
            <p class="font-mono font-bold text-sm sm:text-base capitalize">{{ prayer.label }}</p>
            <p class="font-mono text-lg text-blue-600 font-semibold mt-1">
              {{ timings ? timings[prayer.key] : '--:--' }}
            </p>
          </div>
        </div>
      </template>

    </div>
  </div>
</template>