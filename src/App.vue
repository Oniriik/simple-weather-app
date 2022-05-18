<template>
  <div
    id="page-container"
    class="
      w-screen
      h-screen
      bg-cover
      flex
      justify-center
      flex-col
      font-jakarta
      duration-1000
    "
  >
    <div
      class="weather min-w-400 m-auto bg-black/70 w-auto h-auto rounded-3xl p-8"
      v-if="initialized"
    >
      <div class="search flex flex-col">
        <div class="flex gap-3 items-center justify-center">
          <input
            v-model="fieldCity"
            type="text"
            class="rounded-3xl w-40 pl-3 pr-3 pt-1 pb-1 outline-0"
            @keyup.enter="updateWeather(fieldCity)"
          />
          <img
            src="@/assets/search.png"
            alt="search"
            class="w-4 h-4 invert cursor-pointer"
            @click="updateWeather(fieldCity)"
          />
        </div>
        <p class="text-center text-red-600 text-xs mt-1">{{ message }}</p>
      </div>
      <div
        class="
          display-weather
          mt-4
          text-white
          flex flex-row
          gap-5
          justify-between
        "
      >
        <div>
          <p class="text-xl">Weather in</p>
          <p class="capitalize text-5xl">{{ weather?.name }}</p>
          <div class="text-xs mt-7">
            <p>Feels {{ Math.round(weather.main?.feels_like) }}°</p>
            <p>Humidity {{ weather.main?.humidity }}%</p>
            <p>Wind {{ weather.wind?.speed }} km/h</p>
          </div>
        </div>
        <div class="text-right">
          <p class="text-7xl mt-2">{{ Math.round(weather.main?.temp) }}°</p>
          <img :src="weather.weatherIcon" alt="weatherIcon" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  components: {},
  data() {
    return {
      initialized: false,
      apiKey: process.env.VUE_APP_WEATHER_API_KEY,
      message: "",
      fieldCity: "",
      weather: {},
      background: {
        orientation: "landscape",
      },
    };
  },
  methods: {
    getWeather(city) {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${this.apiKey}`
        )
        .then((res) => {
          this.message = "";
          this.weather = { ...res.data };
          this.weather.weatherIcon = `https://openweathermap.org/img/wn/${res.data.weather[0].icon}@2x.png`;

          fetch(
            `https://source.unsplash.com/1600x900/?${this.weather.weather[0].description}`
          ).then((response) => {
            document.getElementById(
              "page-container"
            ).style.backgroundImage = `url('${response.url}')`;
          });
        })
        .catch(() => {
          this.message = "sorry we dont have infos for this city";
        });
      return;
    },
    updateWeather(city) {
      this.getWeather(city);
      this.fieldCity = "";
    },
    initialize() {
      this.getWeather("paris");
      setTimeout(() => {
        this.initialized = true;
      }, 500);
    },
  },
  created() {
    this.initialize();
  },
};
</script>

<style>
</style>