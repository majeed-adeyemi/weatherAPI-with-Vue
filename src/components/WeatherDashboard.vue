<template>
  <div class="min-h-screen bg-blue-50 flex flex-col items-center justify-center p-4">
    <div class="bg-white shadow-lg rounded-2xl p-6 w-full max-w-md">
      <h1 class="text-2xl font-bold mb-4 text-center text-blue-700">Weather Dashboard</h1>
      <input
        v-model="city"
        @keyup.enter="getWeather"
        placeholder="Enter city name"
        class="w-full p-2 border border-gray-300 rounded mb-4 focus:outline-none focus:ring-2 focus:ring-blue-400"
      />
      <button
        @click="getWeather"
        class="bg-blue-500 text-white px-4 py-2 rounded w-full hover:bg-blue-600"
      >
        Get Weather
      </button>

      <div v-if="weather" class="mt-6">
        <h2 class="text-xl font-semibold text-gray-700">{{ weather.name }}, {{ weather.sys.country }}</h2>
        <p class="text-3xl font-bold text-blue-700">{{ weather.main.temp }}°C</p>
        <p class="text-gray-600">{{ weather.weather[0].description }}</p>
        <div class="mt-2 text-sm text-gray-500">
          <p>Humidity: {{ weather.main.humidity }}%</p>
          <p>Wind: {{ weather.wind.speed }} m/s</p>
        </div>
      </div>

      <div v-if="error" class="mt-4 text-red-500 text-sm text-center">
        {{ error }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const API_KEY = import.meta.env.VUE_APP_API_KEY
const city = ref('')
const weather = ref(null)
const error = ref('')

const getWeather = async () => {
  if (!city.value) return
    console.log(`API URL: https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${API_KEY}&units=metric`);

  try {
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${API_KEY}&units=metric`
    )
    weather.value = response.data
    error.value = ''
  } catch (err) {
    weather.value = null
    error.value = 'City not found. Please try again.'
  }
}
</script>
