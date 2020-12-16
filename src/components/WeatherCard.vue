<template>
  <div class="weather-card">{{ name }}</div>
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
};
</script>

<style></style>
