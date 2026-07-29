<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import navbar from '@/components/navbar.vue'
import SearchBar from '@/components/searchbar.vue'
import AdhanDis from '@/components/adhanDis.vue'

let Cityy
const timings = ref(null)
const hijriDate = ref(null)
const loading = ref(false)
const error = ref(null)

// Fallback location (e.g., Algiers)
const DEFAULT_LAT = 36.7538
const DEFAULT_LNG = 3.0588

// Fetch by City Name
const fetchByCity = async (city) => {
  loading.value = true
  error.value = null
  try {
    const res = await axios.get('https://api.aladhan.com/v1/timingsByCity', {
      params: {
        city: city,
        country: '',
        method: 3 // Egyptian General Authority of Survey
      }
    })
    Cityy=city
    timings.value = res.data.data.timings
    hijriDate.value = res.data.data.date.hijri
  } catch (err) {
    error.value = `Could not find timings for "${city}". Please try another city.`
  } finally {
    loading.value = false
  }
}

// Fetch by Lat/Lng Coordinates
const fetchByCoords = async (lat, lng) => {
  loading.value = true
  error.value = null
  try {
    const res = await axios.get('https://api.aladhan.com/v1/timings', {
      params: {
        latitude: lat,
        longitude: lng,
        method: 3
      }
    })
    Cityy='your location'
    timings.value = res.data.data.timings
    hijriDate.value = res.data.data.date.hijri
  } catch (err) {
    error.value = 'Failed to fetch location timings.'
  } finally {
    loading.value = false
  }
}

// Browser Geolocation
const handleUserLocation = () => {
  if (!navigator.geolocation) {
    error.value = 'Geolocation is not supported by your browser.'
    fetchByCoords(DEFAULT_LAT, DEFAULT_LNG)
    return
  }

  navigator.geolocation.getCurrentPosition(
    (pos) => {
      fetchByCoords(pos.coords.latitude, pos.coords.longitude)
    },
    (err) => {
      console.warn('Geolocation denied or failed. Using fallback location.', err)
      fetchByCoords(DEFAULT_LAT, DEFAULT_LNG)
    }
  )
}

onMounted(() => {
  // Load default location when page loads
  handleUserLocation()
})
</script>

<template>
  <div class="min-h-screen bg-stone-100">
    <navbar />
    <SearchBar 
      @search-city="fetchByCity" 
      @use-location="handleUserLocation" 
    />
    <AdhanDis 
      :cityy="Cityy"
      :timings="timings" 
      :hijri-date="hijriDate" 
      :loading="loading" 
      :error="error" 
    />
  </div>
</template>