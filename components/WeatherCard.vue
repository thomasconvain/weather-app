<template>
  <div>
    <div class="bg-white overflow-hidden rounded-2xl divide-y divide-gray-100">
    <div class="px-4 py-5 sm:px-6">
      <span class="inline-flex items-center px-2.5 py-0.5 rounded-md text-sm font-medium bg-neutral-100 text-neutral-800">
        <svg class="-ml-0.5 mr-1.5 h-2 w-2 text-neutral-400" fill="currentColor" viewBox="0 0 8 8">
          <circle cx="4" cy="4" r="3" />
        </svg>
        <h2 class="font-semibold">{{ weatherData.name }}, {{ weatherData.sys.country }}</h2>
    </span>
    </div>
    <div class="px-4 py-5 sm:p-6">
      <div class="grid grid-cols-2 flex items-center">
        <img :src="getWeatherIconUrl(weatherData.weather[0].icon,'@4x')" alt="Weather Icon" />
        <p class="text-3xl">{{ Math.round(weatherData.main.temp) }}°C</p>
      </div>
    </div>
    <div class="px-4 py-4 sm:px-6 divide-y divide-gray-50 ">
      <!-- Content goes here -->
      <!-- We use less vertical padding on card footers at all sizes than on headers or body sections -->
      <div v-for="item in weatherForecast" :key="item.dt" class="my-4 grid grid-cols-2 flex items-center ">
        <div>{{ item.dayOfWeek }}</div>
        <div class="flex items-center">
          <div><img :src="getWeatherIconUrl(item.weather[0].icon, '')"/></div>
          <div>{{ Math.round(item.main.temp) }}ºC</div>
        </div>
    </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  props: {
    weatherData: {
      type: Object,
      required: true,
    },
    weatherForecast: {
      type: Object,
      required: true,
    }
  },
  methods: {
    getWeatherIconUrl(iconCode, size) {
      // Reemplaza '10d' con el código de icono proporcionado por la API
      // Puedes adaptar este código según tus necesidades
      return `https://openweathermap.org/img/wn/${iconCode}${size}.png`;
    },
  },
};
</script>

<style scoped>
/* Estilos del componente */
</style>