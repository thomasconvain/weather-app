<template>
  <div>
    <h1>Météo Radio Chilmark</h1>
    <div v-if="weatherDataList">
      <WeatherCard v-for="weatherData in weatherDataList" :key="weatherData.id" :weatherData="weatherData" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      weatherDataList: null,
    };
  },
  async mounted() {
    try {
      const cities = ['2996944', '2614626', '6438886', '3871336'];  // Puedes modificar esta lista según tus necesidades
      const cityString = cities.join(',');
      console.log(cityString)
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/group?id=${cityString}&units=metric&appid=eeef46dc73575829789f118bf48aea06`
      );
      const data = await response.json();
      this.weatherDataList = data.list;
    } catch (error) {
      console.error('Error al obtener datos meteorológicos', error);
    }
  },
};
</script>
