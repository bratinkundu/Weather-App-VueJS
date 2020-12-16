<template>
  <div class="weather-card">{{ act_temp(temp) }}</div>
</template>

<script>
export default {
  name: "weather-card",
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
  },
  methods: {
    act_temp: function(temp) {
      return Math.round(temp - 273);
    },
  },
  computed: {},
};
</script>

<style lang="scss" scoped>
.weather-card {
  flex-basis: 500px;
  height: 400px;
  background: #414141;
}
</style>
