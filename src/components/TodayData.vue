<template>
  <div class="today-data">
    <div class="main-temp">
      <h2 class="temp">{{ temp }}</h2>
      <div class="unit-selector">
        <span class="deg">°<span class="temp_type">C</span></span>
        <!-- <span class="deg">°<span class="temp_type">F</span></span> -->
      </div>
    </div>
    <div class="other-info">
      <h6 class="other-info-text">
        Min Temp: <span class="info-value">{{ min_temp }}°c</span>
      </h6>
      <h6 class="other-info-text">
        Max Temp: <span class="info-value">{{ max_temp }}°c</span>
      </h6>
      <h6 class="other-info-text">
        WInd speed: <span class="info-value">{{ speed }} km/h</span>
      </h6>
      <!-- <h6 class="other-info-text">Min Temp: <span class="info-value">{{ min_temp }}</span></h6> -->
    </div>
  </div>
</template>

<script>
export default {
  name: "todayDataComponent",
  props: {
    temp: Number,
    min_temp: Number,
    max_temp: Number,
    speed: Number,
  },
  data() {
    return {
      temp_unit: true,
    };
  },
};
</script>

<style lang="scss" scoped>
.today-data {
  min-height: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  width: 100%;
  align-items: center;
  gap: 100px;
  margin: 0 15px;
}
.main-temp {
  display: flex;
  align-items: flex-start;
}
.unit-selector {
  margin-top: 30px;
  font-size: 3rem;
}
.other-info {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
}
.other-info-text {
  font-size: 1.4rem;
  font-weight: 100;
}
.info-value {
  font-size: 1em;
  font-weight: 900;
}
.temp {
  font-size: 12rem;
}
.temp_type {
  font-size: 0.785em;
}
</style>
