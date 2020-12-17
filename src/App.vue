<template>
  <div id="app" v-if="weatherData">
    <Card
      :name="weatherData.name"
      :country="weatherData.sys.country"
      :sunrise="weatherData.sys.sunrise"
      :sunset="weatherData.sys.sunset"
      :temp="weatherData.main.temp"
      :max_temp="weatherData.main.temp_max"
      :min_temp="weatherData.main.temp_min"
      :windSpeed="weatherData.wind.speed"
      :weather_desc="weatherData.weather.description"
      :humidity="weatherData.main.humidity"
      :time="weatherData.dt"
      :graphData="graphData"
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
      hourlyData: "http://api.openweathermap.org/data/2.5/forecast?",
      weatherData: null,
      graphData: Array,
    };
  },
  methods: {
    // Async functions (API calls)
    async getWeatherData(cityName = "London") {
      try {
        const response = await fetch(
          `${this.weatherURL}q=${cityName}&appid=${this.OPEN_WEATHER_API}`
        );
        this.weatherData = await response.json();
      } catch (err) {
        console.log(err);
      }
    },

    async getHourlyData(cityName = "London") {
      try {
        const response = await fetch(
          `${this.hourlyData}q=${cityName}&appid=${this.OPEN_WEATHER_API}`
        );
        this.graphData = this.filterDateAndTemp(await response.json());
        console.log(this.graphData);
      } catch (err) {
        console.log(err);
      }
    },

    // Synconous functions
    filterDateAndTemp(data) {
      const newData = data.list.filter((ele) => {
        return new Date(ele.dt * 1000).getDate() === new Date().getDate();
      });
      const temps = newData.map((ele) => Math.round(ele.main.temp - 273));
      const times = newData.map((ele) =>
        new Date(ele.dt * 1000).toLocaleTimeString()
      );
      return {
        temps,
        times,
      };
      // return data.list.slice(1, 5).map((ele) => {
      //   return {
      //     timestamp: new Date(ele.dt * 1000).toLocaleTimeString(),
      //     temp: Math.round(ele.main.temp - 273),
      //   };
      // });
      // return data.list
      //   .filter((ele) => {
      //     return new Date(ele.dt * 1000).getDate() === new Date().getDate();
      //   })
      //   .map((ele) => {
      //     return {
      //       timestamp: new Date(ele.dt * 1000).toLocaleTimeString(),
      //       temp: Math.round(ele.main.temp - 273),
      //     };
      //   });
    },
  },
  computed: {},
  mounted() {
    this.getWeatherData();
    this.getHourlyData();
  },
};
</script>

<style lang="scss">
#app {
  min-height: 100vh;
  width: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
