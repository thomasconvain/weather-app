<template>
  <div class="p-8 h-full lg:h-screen bg-slate-100">
    <div>
      <h1 class="text-slate-900 text-3xl flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-8 h-8 mr-1">
          <path stroke-linecap="round" stroke-linejoin="round" d="M12 3v2.25m6.364.386-1.591 1.591M21 12h-2.25m-.386 6.364-1.591-1.591M12 18.75V21m-4.773-4.227-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0Z" />
        </svg>
        Météo Radio Chilmark
      </h1>
      <div v-if="groupWeatherData" class="mt-4 grid md:grid-cols-2  lg:grid-cols-4 gap-6">
        <WeatherCard v-for="weatherData in groupWeatherData" :key="weatherData.id" :weatherData="weatherData" :weatherForecast="getDailyForecast(forecastData[weatherData.id])" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cities: ['2996944', '2614626', '6438886', '3871336'],
      forecastData: {},
      groupWeatherData: null,
    };
  },
  methods: {
    getDailyForecast(forecast) {
      // Filtrar los datos para obtener solo un timestamp por día a las 12:00 PM
      const dailyForecast = forecast.filter((item) => {
        const date = new Date(item.dt_txt);
        return date.getHours() === 15 && date.getMinutes() === 0;
      });

      // Obtener el nombre del día de la semana
      dailyForecast.forEach((item) => {
        const date = new Date(item.dt_txt);
        const daysOfWeek = ['Dimanche', 'Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi'];
        item.dayOfWeek = daysOfWeek[date.getDay()];
      });

      return dailyForecast;
    },
  },
  async mounted() {
    try {
      // 1. Obtener previsión del tiempo para varias ciudades
      const forecasts = await Promise.all(
        this.cities.map(async (city) => {
          const response = await fetch(
            `https://api.openweathermap.org/data/2.5/forecast?id=${city}&units=metric&appid=eeef46dc73575829789f118bf48aea06&cnt=40`
          );
          const data = await response.json();
          return { city, forecast: data.list };
        })
      );

      const forecastData = {};
      forecasts.forEach(({ city, forecast }) => {
        forecastData[city] = forecast;
      });
      this.forecastData = forecastData;

      // 2. Obtener información actual del tiempo para el grupo de ciudades
      const cityString = this.cities.join(',');
      const groupResponse = await fetch(
        `https://api.openweathermap.org/data/2.5/group?id=${cityString}&units=metric&appid=eeef46dc73575829789f118bf48aea06`
      );
      const groupData = await groupResponse.json();
      this.groupWeatherData = groupData.list;
    } catch (error) {
      console.error('Error al obtener datos meteorológicos', error);
    }
  },
};
</script>
