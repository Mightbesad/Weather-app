<template>
  <div class="container p-0">
    <div class="d-flex">
      <div class="card main-div w-100">
        <div class="p-3">
          <h3 class="mb-1 day">{{day}}</h3>
          <p class="text-light date mb-0">{{date}}</p>
          <small>{{time}}</small>
          <h2 class="place"><i class="fa-solid fa-location-crosshairs">{{name}}<small>{{country}}</small></i></h2>
          <div class="temp">
            <h1 class="weather-temp">{{temperature}}&deg;</h1>
            <h2 class="text-light">{{description}} <img :src="iconURl" alt="Loading..."> </h2>
          </div>
        </div>
      </div>

      <div class="card card-2 w-100">
        <table class="m-4">
          <tbody>
            <tr>
              <th>Sea Level</th>
              <td v-if="sea_level > 0">{{sea_level}}</td>
              <td v-else>Null</td>
            </tr>
            <tr>
              <th>Humidity</th>
              <td>{{humidity}}</td>
            </tr>
            <tr>
              <th>Wind</th>
              <td>{{wind}}</td>
            </tr>
          </tbody>
        </table>

        <DaysWeather :cityname="cityname"></DaysWeather>

        <div id="div_form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input type="button" value="Change Location" @click="changeLocation" class="btn change-btn btn-primary">
          </form>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import DaysWeather from './DaysWeather.vue';
import axios from 'axios';
export default {
  name: 'myWeather',
  components: {
    DaysWeather
  },
  props: { 
    city: String,
  },
  data() {
    return {
      cityname: this.city,
      temperature: null,
      description: null,
      date: null,
      time: null,
      country: null,
      iconURl: null,
      day: null,
      name: null,
      sea_level: null,
      wind: null,
      humidity: null,
      monthNames: ["January","February","March","April","May","June","July",
            "August","September","October","November","December"],
            
    }
  },
  methods: {
    changeLocation(){
      window.location.reload();

    }
  },
  async created() {
    try {
      const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&APPID=3c5abfbcbe2f0611c805db0a8285c619`);
      const weatherData = response.data;
      this.temperature = Math.round(weatherData.main.temp);
      this.description = weatherData.weather[0].description;
      this.name = weatherData.name;
      this.country = weatherData.sys.country;
      this.wind = weatherData.wind.speed;
      this.humidity = weatherData.main.humidity;
      this.sea_level = weatherData.main.sea_level;


      this.iconURl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`
      const d = new Date();
      this.day = this.monthNames[d.getMonth()];
      this.date = d.getDate() + ' ' + this.monthNames[d.getMonth()] + ' ' + d.getFullYear();
      this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
      console.log(weatherData);
    } catch (error) {
      console.error(error);
    }
  },
}
</script>

<style>
body {
  background-color: #343d4b;
}

.weather-temp {
  margin: 0;
  font-weight: 100;
  font-size: 4rem;
}

h2.mb-1.day {
  font-size: 3rem;
  font-weight: 400;

}

.card.main-div {
  border-radius: 20px;
  color: #fff;
  background-image: url("https://images.unsplash.com/photo-1510414842594-a61c69b5ae57?auto=format&fit=crop&q=60&w=700&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MzV8fGJlYWNofGVufDB8fDB8fHww");
  background-size: cover;
  background-position: center;
  background-blend-mode: overlay;
  background-color: rgba(0, 0, 0, 0.5);
  background-repeat: no-repeat;

}

.temp {
  position: absolute;
  bottom: 0;
}

.card.main-div:hover {
  transform: scale(1.1);
  transition: transition 0.5s ease;
  z-index: 1;
}

.card.card-2 {
  background-color: #212730;
  border-radius: 20px;
}

.card-details {
  margin-left: 19px;

}

.h1_left {
  position: absolute;
  bottom: 25px;
  left: 16px;
  font-size: 3vw;
  line-height: 1.2;
}

.h1_left {
  position: absolute;
  left: 16px;
  font-size: 2vw;
  line-height: 0.5;
}

.h1_left small {
  font-size: 1rem;
}

table {
  position: relative;
  left: 15px;
  border-collapse: separate;
  border-spacing: 15px;
  width: 85%;
  text-align: left;
  max-width: 600px;
  margin: 0 auto;
}

th,
td {
  font-size: 18px;
  color: #fff;

}

td {
  text-align: right;
}

table,
tr:hover {
  color: red;
}

.change-btn {
  background-image: linear-gradient(to right, cyan, magenta);
}

.change-btn:hover {
  transform: scale(0.9);
  transition: transform 0.1s ease;
}

</style>
