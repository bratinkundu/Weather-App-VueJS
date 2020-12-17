<template>
  <div class="weather-card">
    <h1 class="location">{{ name }}, {{ country }}</h1>
    <TodayData
      :temp="act_temp(temp)"
      :min_temp="act_temp(min_temp)"
      :max_temp="act_temp(max_temp)"
      :speed="windSpeed"
    />
    <LineChart
      class="chart"
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
};
</script>

<style lang="scss" scoped>
@import "../assets/styles/common.scss";
.weather-card {
  @include flex-center;
  flex-direction: column;
  width: clamp(300px, 50%, 80%);
  // min-height: 100vh;
}
.location {
  font-size: 3rem;
}
.chart {
  display: flex;
  justify-content: center;
  width: 100%;
  // margin: 0 5%;
}
</style>
