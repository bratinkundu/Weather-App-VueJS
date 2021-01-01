<template>
  <div id="app" :class="dayOrNight" v-if="weatherData">
    <LocationForm
      @new-text-location="getWeatherData"
      @new-geo-location="getWeatherData"
    />
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
import LocationForm from "./components/LocationForm";

export default {
  name: "App",
  components: {
    Card,
    LocationForm,
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
    async getWeatherData(data) {
      try {
        if (!data) data = "London";

        const response =
          typeof data === typeof ""
            ? await fetch(
                `${this.weatherURL}q=${data}&appid=${this.OPEN_WEATHER_API}`
              )
            : await fetch(
                `${this.weatherURL}lat=${data.lati}&lon=${data.long}&appid=${this.OPEN_WEATHER_API}`
              );

        this.weatherData = await response.json();
        this.getHourlyData(data);
        console.log(this.weatherData);
      } catch (err) {
        console.log(err);
      }
    },

    async getHourlyData(data) {
      try {
        if (!data) data = "London";
        console.log(data);

        const response =
          typeof data === typeof ""
            ? await fetch(
                `${this.hourlyData}q=${data}&appid=${this.OPEN_WEATHER_API}`
              )
            : await fetch(
                `${this.hourlyData}lat=${data.lati}&lon=${data.long}&appid=${this.OPEN_WEATHER_API}`
              );
        const respData = await response.json();
        console.log(respData);
        this.graphData = this.filterDateAndTemp(respData);
      } catch (err) {
        console.log(err);
      }
    },

    async getInputWeatherData(data) {
      console.log(data);
    },
    // Synconous functions
    filterDateAndTemp(data) {
      // const newData = data.list.filter((ele) => {
      //   return new Date(ele.dt * 1000).getDate() === new Date().getDate();
      // });
      const newData = data.list.slice(1, 5);
      const temps = newData.map((ele) => (ele.main.temp - 273).toFixed(1));
      const times = newData.map((ele) => {
        const time = new Date(ele.dt * 1000);
        return `${
          time.getHours() < 12 ? time.getHours() : time.getHours() - 12
        }:${time.getMinutes()} ${time.getHours() > 12 ? "AM" : "PM"}`;
      });
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
  computed: {
    dayOrNight: function() {
      return new Date(this.weatherData.dt * 1000).getHours() > 12
        ? "night"
        : "day";
    },
  },
  mounted() {
    this.getWeatherData();
    this.getHourlyData();
    // this.getUserLocation();
  },
};
</script>

<style lang="scss">
#app {
  min-height: 100vh;
  width: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.day {
  // background: yellow;
  color: #414141;
}
.night {
  background: #414141;
  color: white;
}
</style>
