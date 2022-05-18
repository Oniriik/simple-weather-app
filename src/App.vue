<template>
<div id="page-container" class="w-screen h-screen bg-cover flex justify-center flex-col">
    <div class="weather m-auto bg-black/70 w-96 h-80 rounded-3xl p-3">
      <div class="searchCity flex gap-3 items-center justify-center">
        <input type="text" name="city" id="city" class="rounded-3xl pl-3 pr-3 pt-1 pb-1">
        <img src="@/assets/search.png" alt="search" class="w-4 h-4 invert">

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
      weather: {
        time: "night",
        temp: "18°",
        current_weather: "sunny",
        humidity: "8%",
        wind: "5Km/h"
      },
      background: {
        orientation: "landscape"
      },
    };
  },
  methods: {
    getWeather() {
      console.log(process.env.VUE_APP_WEATHER_API_KEY);
      axios
        .get("https://api.openweathermap.org/data/2.5/weather", {
          params: {
            lat: this.city.lat,
            lon: this.city.long,
            appid: this.apiKey,
            
          },
        })
        .then((res) => {
          console.log(res);
          return res;
        });
    },
    async getBackgroundImg(){
      var query = encodeURI(`${this.weather.current_weather} ${this.weather.time}`);
      await fetch(`https://source.unsplash.com/1600x900/?${query}`).then((response) => {   
      document.getElementById('page-container').style.backgroundImage = `url('${response.url}')`;
  }) 
    }
  },
  async mounted() {
    await this.getBackgroundImg();
    this.getWeather()
  },
};
</script>

<style>
</style>