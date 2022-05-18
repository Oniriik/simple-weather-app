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
      duration-500
    "
  >
    <div class="weather m-auto bg-black/70 w-96 h-auto rounded-3xl p-8">
      <div class="search flex gap-3 items-center justify-center">
        <input
          type="text"
          name="city"
          id="city"
          class="rounded-3xl w-40 pl-3 pr-3 pt-1 pb-1 outline-0"
        />
        <img src="@/assets/search.png" alt="search" class="w-4 h-4 invert" />
      </div>
      <div
        class="display-weather mt-4 text-white flex flex-row justify-between"
      >
        <div>
          <p class="text-xl">Weather in</p>
          <p class="capitalize text-5xl">{{ city.name }}</p>
          <div class="text-xs mt-7">
            <p>Feels {{ Math.round(weather.main.feels_like) }}°</p>
            <p>Humidity {{weather.main.humidity}}%</p>
            <p>Wind {{weather.wind.speed}} km/h</p>
          </div>
        </div>
        <div>
          <p class="text-7xl mt-2">{{ Math.round(weather.main.temp) }}°</p>
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
      apiKey: process.env.VUE_APP_WEATHER_API_KEY,
      fieldCity: "",
      city: {
        name: "paris",
        lat: "48.866667",
        long: "2.333333",
      },
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
          console.log(res);
          this.weather = { ...res.data };
          this.weather.weatherIcon = `https://openweathermap.org/img/wn/${res.data.weather[0].icon}@2x.png`;
          console.log(
            `https://openweathermap.org/img/wn/${res.data.weather[0].icon}@2x.png`
          );
        })
        .catch((err) => {
          console.error(err);
        });
    },
    getBackgroundImg() {
      var query = encodeURI(
        `${this.weather.current_weather} ${this.weather.time}`
      );
      fetch(`https://source.unsplash.com/1600x900/?${query}`).then(
        (response) => {
          document.getElementById(
            "page-container"
          ).style.backgroundImage = `url('${response.url}')`;
        }
      );
    },
  },
  async beforeMount() {
    this.getWeather(this.city.name);
    this.getBackgroundImg();
  },
};
</script>

<style>
</style>