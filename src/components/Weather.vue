<template>
  <div class="weather-container" @click="changeMetric" v-if="weatherData">
    <transition name="slide-fade">
      <p v-if="showMetric">{{ Math.round(weatherData.main.temp - 273) }}° c</p>
    </transition>
    <transition name="slide-fade">
      <p v-if="!showMetric">
        {{ Math.round((weatherData.main.temp * 9) / 5 - 459.67) }}° f
      </p>
    </transition>
    <div class="vertical-divider"></div>
    <span>{{ weatherData.weather[0].main }}</span>
  </div>
</template>

<script>
import key from "../../keys";
import axios from "axios";
export default {
  name: "Weather",
  props: {
    lock: {
      type: Boolean,
    },
  },
  data() {
    return {
      weatherData: null,
      showMetric: null,
    };
  },
  created() {
    const notFirstTime = !!localStorage.getItem("metric");
    if (notFirstTime) {
      this.showMetric = JSON.parse(localStorage.getItem("metric"));
    } else {
      this.showMetric = true;
    }
  },
  mounted() {
    this.fetchWeather();
  },
  methods: {
    fetchWeather() {
      axios.get("http://ip-api.com/json").then((res) => {
        return axios
          .get(
            `http://api.openweathermap.org/data/2.5/weather?q=${res.data.city}&appid=${key.weather_api}`
          )
          .then((res) => (this.weatherData = res.data));
      });
    },
    changeMetric() {
      if (this.lock) return;
      this.showMetric = !this.showMetric;
      localStorage.setItem("metric", this.showMetric);
    },
  },
};
</script>

<style scoped>
.weather-container {
  cursor: pointer;
  color: white;
  font-weight: 600;
  display: flex;
  justify-content: space-between;
}

.vertical-divider {
  opacity: 0.3;
  align-self: stretch;
  width: 1px;
  background-color: white;
}

.weather-container > p {
  padding: 0;
}

.weather-container > * {
  margin: 0em 1em 0em 0em;
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateX(-10px);
  opacity: 0;
}
</style>