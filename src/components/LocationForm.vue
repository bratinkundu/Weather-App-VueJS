<template>
  <div class="user-input">
    <form @submit="onTextSubmit($event)">
      <input
        class="input-text"
        type="text"
        name="city"
        autocomplete="off"
        aria-autocomplete="off"
        placeholder="City Name"
        v-model="cityName"
      />
      <button class="btn btn-primary">
        Search
      </button>
      <button class="btn btn-secondary" @click="onLocationSubmit($event)">
        Locate Me
      </button>
    </form>
    <span v-if="error > 0" class="error-text">Enter a valid input</span>
  </div>
</template>

<script>
export default {
  name: "UserInput",
  data: function() {
    return {
      cityName: "",
      error: false,
    };
  },
  methods: {
    onTextSubmit(e) {
      e.preventDefault();
      !this.cityName ? (this.error = true) : (this.error = false);
      this.$emit("new-text-location", this.cityName);
    },
    onLocationSubmit(e) {
      if (e) e.preventDefault();
      const data = this.getUserLocation();
      this.$emit("new-geo-location", data);
    },
    getUserLocation() {
      navigator.geolocation.getCurrentPosition((position) => {
        console.log(position.coords.latitude, position.coords.longitude);
        return {
          lati: position.coords.latitude,
          long: position.coords.longitude,
        };
      });
    },
  },
  created() {
    this.onLocationSubmit();
  },
};
</script>

<style lang="scss" scoped>
.user-input {
  display: flex;
  flex-direction: column;
  margin: 0 0 20px 0;
}
.input-text {
  background: #e3e3e3;
  border: none;
  padding: 10px 15px;
  border-radius: 0;
  flex-basis: 200px;
  flex-grow: 1;
  flex: 1;
}

.btn {
  padding: 10px 15px;
  border: 0;
  border-radius: 0;
}

.btn-primary {
  background: #e3e3e3;
}

.btn-secondary {
  background: #3e3e3e;
  color: #e3e3e3;
}

.error-text {
  margin: 10px 0;
  color: #fe391a;
}
</style>
