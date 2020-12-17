<template>
  <div class="weather-card" :class="dayOrNight">
    <h1 class="location">{{ name }}, {{ country }}</h1>
    <TodayData
      :temp="act_temp(temp)"
      :min_temp="act_temp(min_temp)"
      :max_temp="act_temp(max_temp)"
      :speed="windSpeed"
    />
    <LineChart
      :data="graphData.temps"
      :label="graphData.times"
      v-if="graphData.times"
    />
  </div>
</template>

<script>
import TodayData from "./TodayData";
import LineChart from "./HourlyGraph";

export default {
  name: "weather-card",
  components: {
    TodayData,
    LineChart,
  },
  props: {
    name: String,
    country: String,
    temp: Number,
    max_temp: Number,
    min_temp: Number,
    humidity: Number,
    windSpeed: Number,
    weather_desc: String,
    sunrise: Number,
    sunset: Number,
    time: Number,
    graphData: [Object, Function, undefined],
  },
  methods: {
    act_temp: function(temp) {
      return Math.round(temp - 273);
    },
  },
  computed: {
    dayOrNight: function() {
      return new Date(this.time).getHours() > 12 ? "night" : "day";
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/common.scss";
.weather-card {
  @include flex-center;
  flex-direction: column;
  width: 100%;
  min-height: 100vh;
}
.location {
  font-size: 3rem;
}
.day {
  background: yellow;
  color: #414141;
}
.night {
  background: #414141;
  color: white;
}
</style>
