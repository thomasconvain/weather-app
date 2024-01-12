<template>
  <div>
    <h2>{{ weatherData.name }}</h2>
    <img :src="getWeatherIconUrl(weatherData.weather[0].icon)" alt="Weather Icon" />
    <p>{{ weatherData.main.temp }}°C</p>
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
      return `http://openweathermap.org/img/w/${iconCode}.png`;
    },
  },
};
</script>

<style scoped>
/* Estilos del componente */
</style>