<template>
  <div>
    <div class="bg-white overflow-hidden shadow rounded-lg divide-y divide-gray-200">
    <div class="px-4 py-5 sm:px-6">
      <h2 class="font-semibold">{{ weatherData.name }}, {{ weatherData.sys.country }}</h2>
    </div>
    <div class="px-4 py-5 sm:p-6">
      <div class="grid grid-cols-2 flex items-center">
        <img :src="getWeatherIconUrl(weatherData.weather[0].icon)" alt="Weather Icon" />
        <p>{{ weatherData.main.temp }}°C</p>
      </div>
    </div>
    <div class="px-4 py-4 sm:px-6">
      <!-- Content goes here -->
      <!-- We use less vertical padding on card footers at all sizes than on headers or body sections -->
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
  },
  methods: {
    getWeatherIconUrl(iconCode) {
      // Reemplaza '10d' con el código de icono proporcionado por la API
      // Puedes adaptar este código según tus necesidades
      return `http://openweathermap.org/img/wn/${iconCode}@4x.png`;
    },
  },
};
</script>

<style scoped>
/* Estilos del componente */
</style>