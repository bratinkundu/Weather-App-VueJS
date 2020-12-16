<template>
  <div class="app" v-if="weatherData">
    <Card
      :name="weatherData.name"
      :country="weatherData.sys.county"
      :sunrise="weatherData.sys.sunrise"
      :sunset="weatherData.sys.sunset"
      :temp="weatherData.main.tmep"
      :max_temp="weatherData.main.max_temp"
      :min_temp="weatherData.main.min_temp"
      :windSpeed="weatherData.wind.speed"
      :weather_desc="weatherData.weather.description"
      :humidity="weatherData.main.humidity"
    />
  </div>
</template>

<script>
require("dotenv").config();
import Card from "./components/WeatherCard";
export default {
  name: "App",
  components: {
    Card,
  },
  data: () => {
    return {
      OPEN_WEATHER_API: "9e9d9499cf2a6979cddba99d7199569f",
      weatherURL: "http://api.openweathermap.org/data/2.5/weather?",
      weatherData: null,
    };
  },
  methods: {
    async getWeatherData() {
      try {
        const response = await fetch(
          `${this.weatherURL}q=London&appid=${this.OPEN_WEATHER_API}`
        );
        const data = await response.json();
        console.log(data);
        this.weatherData = data;
      } catch (err) {
        console.log(err);
      }
    },
    getTempValue(tempValue) {
      return tempValue - 273;
    },
  },
  computed: {},
  mounted() {
    this.getWeatherData();
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
