<template>
<p>{{this.city}}</p>
<p>{{this.weather}}</p>
</template>

<script>
import axios from 'axios'


export default {
  components: {
  },
  data() {
    return {
      apiKey: process.env.VUE_APP_API_KEY,
      city: {
        city: 'paris',
        lat: '48.866667',
        long :'2.333333'
      },
      weather: undefined
    },
  },
  methods: {
    getWeather(){
      axios.get('https://api.openweathermap.org/data/3.0/onecall', {
      params: {
        lat: this.city.lat,
        lon: this.city.long,
        appid: this.apiKey
      }
    })
    .then(res => {
      console.log(res);
      return res
    })
  },
    }
  },
  beforeMount() {
    this.getWeather().then((weather)=>{
      this.weather = {... weather}
    })
}
</script>

