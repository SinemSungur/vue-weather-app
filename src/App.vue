<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          v-model="searchInput"
          @keypress.enter="searchForWeather"
          class="search-input"
          placeholder="Search..."
        />
      </div>
      <div class="weather-wrapper" v-if="typeof weather.main !== 'undefined'">
        <div class="location">
          {{ weather.name }}, {{ weather.sys.country }}
        </div>
        <div class="date">{{ dateSetting() }}</div>
        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp) }}°C
            <span>Feels like {{ Math.round(weather.main.feels_like) }}°C</span>
          </div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      api_key: "0dbce00667f28ef5182a951a9a48f7d3",
      url_base: "https://api.openweathermap.org/data/2.5/",
      searchInput: "",
      weather: {},
    };
  },
  methods: {
    searchForWeather() {
      axios
        .get(
          `${this.url_base}/weather?q=${this.searchInput}&units=metric&APPID=${this.api_key}`
        )
        .then((response) => {
          this.weather = response.data;
          console.log("response.data", response);
        });
    },
    dateSetting() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
  computed:{
    weatherBeLike(){
      if(typeof this.weather.main !=='undefined' && this.weather.main.temp > 16){
        return 'sunny'
      }else if(this.weather.weather[0].main === 'cloud'){
        return 'cloudy'
      }else{
        return 'cold'
      }
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  transition: 0.4s;
}
#app {
  background-image: url("./assets/sunny.jpg");
  background-size: cover;
  background-position: center;
}
#app.sunny{
  background-image: url('./assets/sunny.jpg');
}
#app.cloudy{
  background-image: url('./assets/cloudy.jpg');
}
#app.cold{
  background-image: url('./assets/cold.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.4)
  );
  cursor: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAPCAMAAAAxmgQeAAAACVBMVEX///8AAAD///9+749PAAAAAXRSTlMAQObYZgAAAD5JREFUGNN1j9EKADAIAnP//9GjFq6w7iU4DNRsB06cb44TJ+UzYUcniqnqoA6ag/62JhjKIU0rxx1lKveuXJ5UAQs/G/2vAAAAAElFTkSuQmCC),
    auto;
  font-family: "montserrat" sans-serif;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-input {
  display: block;
  width: 100%;
  padding: 15px;
  color: #fff;
  font-size: 20px;
  border: none;
  background: none;
  outline: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 0px 15px 0px 15px;
}

.search-box .search-input:hover {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  border-radius: 15px 0px 15px 0px;
  cursor: pointer;
}
.location {
  font-size: 32px;
  font-weight: 600;
  text-align: center;
  color: #fff;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.2);
}
.date {
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  color: #fff;
}
.weather-box {
  text-align: center;
}
.temp {
  display: inline-block;
  padding: 10px 20px;
  color: #fff;
  font-weight: 900;
  font-size: 102px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.2);
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
}
.temp span {
  font-size: 16px;
}
.weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.2);
}
</style>
