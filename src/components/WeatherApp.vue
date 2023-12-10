<template>
  <div id="app" class="App">
    <div class="main" v-bind:class="{ warm: data.state_weather }">
      <div class="search-box">
        <input class="search-bar" type="text" placeholder="Search..." v-model="data.city" @keyup.enter="getApi()">
        <div>
          <div class="header" v-if="data.weather">
            <h1>{{ data.weather.name }}</h1>
            <h3>{{ new Date().toLocaleString() }}</h3>
            <p>Designed by Ahmet Çakıcı</p>
          </div>
          <div class="temp" v-if="data.weather">
            <h2>{{ Math.round(data.weather.current.temp_c) }}&deg;</h2>
          </div>
          <div class="state" v-if="data.weather">
            <h3>{{ data.weather.current.condition.text }}</h3>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'WeatherApp',
  props: {},
  data() {
    return {
      data: {
        weather: null,
        city: '',
        key: 'c1b0e96377584c0aada75907231409',
        current_day: '',
        state_weather: false
      }
    };
  },
  mounted: async function () {
    this.getApi();
  },
  methods: {
    async getApi() {
      if (this.data.city === '') {
        this.data.city = 'Ankara';
      }
      const cityName = encodeURIComponent(this.data.city); 
      const getWeather = await axios.get(
        `http://api.weatherapi.com/v1/current.json?key=${this.data.key}&lang=tr&q=${cityName}&units=metric`
      );
      this.data.weather = getWeather.data;
      this.data.city = '';
      if (this.data.weather && this.data.weather.current.temp_c > 24) {
        this.data.state_weather = true;
      } else {
        this.data.state_weather = false;
      }
      console.log(this.data.weather);
    }
  }
};
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-image: url("../assets/weatherPhoto.jpg"); 
  background-repeat: no-repeat;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

.main {
  background-image: linear-gradient(to bottom, rgba(18, 75, 156, 0.3), rgba(2, 15, 22, 0.75));
  width: 100%;
  height: 100%;
}
.main.warm{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(248, 8, 56, 0.3), rgba(246, 6, 6, 0.75));
}
.search-box {
  width: 100%;
  left: 300px;
  margin: 240px;
  position: relative;
}

.search-box .search-bar {
  display: block;
  width: 20%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 8px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px;
  background-color: rgba(255, 255, 255, 0.75);
}

.header {
  padding: 20px;
  font-size: 20px;
  color: azure;
  box-shadow: rgba(3, 3, 3, 0.3);
}
.temp{
  display: inline-block;
  padding: 10px 25px;
  color:#FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.state{
  position: absolute;
  color:#FFF;
  font-size: 48px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  text-align:center;
}
</style>
