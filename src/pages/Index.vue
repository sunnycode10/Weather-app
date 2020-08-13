<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
    <q-input
        v-model="search"
        placeholder="Search"
        dark
        borderless
        @keyup.enter="getWeatherBySearch"
      >
        <template v-slot:before>
          <q-icon name="my_location" @click="getLocation"/>
        </template>

        <template v-slot:append>
        <q-btn
          round
          dense
          flat
          icon="search"
          @click="getWeatherBySearch"
         />
        </template>
      </q-input>
    </div>
    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light  ">
          {{weatherData.name}}
        </div>
        <div class="text-h6   text-weight-light">
          {{weatherData.weather[0].main}}
        </div>
        <div class="text-h2 text-weight-thin q-my-lg relative-position">
          <span>{{weatherData.main.temp}}</span>
          <span class="text-h4 degree relative-position">&deg;C</span>
        </div>
      </div>
      <div class="col text-center">
        <img :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`" />
      </div>
    </template>
    <template v-else>
      <div class="col colunm text-white text-center"> 
        <div class="col text-h2 text-weight-thin">
          Quasar <br> weather App
        </div>
      <q-btn
        class="col"
        flat
        @click="getLocation"
      > 
        <q-icon
        left
        size="3em"
        name="my_location"
        />
        <div>Fine
        My
        Location</div>
        </q-btn>
      </div>
    </template>
    <div class="col skyline text-center"></div>
  </q-page>
</template>

<script>
import Axios from 'axios';
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
      weatherData: null,
      lat:null,
      lon:null,
      apiUrl:'https://api.openweathermap.org/data/2.5/weather',
      apiKey: '77483b4f9aa28e3e88f9f092f515ae8b'
    };
  },
  methods:{
     getLocation(params) {
       console.log('getlocation');
        navigator.geolocation.getCurrentPosition(position => {
          this.lat = position.coords.latitude
          this.lon = position.coords.longitude
        console.log('position', position);
          this.getWeatherByCoords()
      })
    },
    getWeatherByCoords(){
      console.log('getting weather coordinate')
      this.$axios.get(`${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`).then(response =>{
       this.weatherData = response.data
      })
    },
    getWeatherBySearch(){
      this.$axios.get(`${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`).then(response =>{
       this.weatherData = response.data
      })
    }
  }
};
</script>
<style lang="sass">
.q-page
  background: linear-gradient(to right, #136a8a, #267871)

.degree
  top: -23px
.skyline
  flex: 0 0 100px
  background-image: url('https://cdn.pixabay.com/photo/2017/01/31/00/33/architecture-2022614_1280.png')
  background-size: contain
  background-position: center bottom
</style>
