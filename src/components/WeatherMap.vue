<template>
  <div class="weather-app" :class="changeImage()">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
      </div>
      <div class="weather-wrap" v-if="isWeather">
        <div class="location-box">
          <div class="location">{{ getCountry }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
      </div>
      <div class="weather-box" v-if="isWeather">
        <div class="temp">{{ getTemp }}&ordm;c</div>
        <div class="weather">{{ getWeather }}</div>
      </div>
    </main>
  </div>
</template>
<script>
export default {
  name: 'WeatherMap',
  data() {
    return {
      api_key: 'aa032ccbef273199c3a713fc6a3eab58',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let current = new Date();
      let days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Satturday', 'Sunday'];
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

      let day = days[current.getDay()];
      let date = current.getDate();
      let month = months[current.getMonth()];
      let year = current.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
    changeImage() {
      if (typeof this.weather.main != "undefined") {
        if (this.weather.main.temp > 16) {
          return 'warm';
        }
      }
      return 'cold';
    }
  },
  computed: {
    getCountry: function() {
      return this.weather.name + ', ' + this.weather.sys.country;
    },
    getTemp: function() {
      return Math.round(this.weather.main.temp);
    },
    getWeather: function() {
      return this.weather.weather[0].main;
    },
    isWeather() {
      return typeof this.weather.main != "undefined";
    }
  }
}
</script>
<style>
.weather-app {
  background-size: cover;
  ;
  background-position: bottom;
  transition: 0.4s;

}

.cold {
	background-image: url('./../assets/cold-bg.jpg');
}

.warm {
  background-image: url('./../assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
  transition: 0.4s;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
