<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="showWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location" :class="{small:weather.name.length>10 || weather.sys.country.length>10}">
            {{ weather.name }} , {{ weather.sys.country }}
          </div>
          <div class="date"> {{dateBuilder()}} </div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "3e8ba888eaae791c4999c5a4545181f1",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },

  methods: {
    showWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((data) => data.json())
          .then((item) => (this.weather = item));
      }
    },

    dateBuilder() {
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
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let month = months[d.getMonth()];
      let year=d.getFullYear()
      let day=days[d.getDay()]
 
      return `${day}, ${month} ${year}`
    },
  },


};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100vh;
  background: url("https://images.unsplash.com/photo-1580193769210-b8d1c049a7d9?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MTh8fHdlYXRoZXJ8ZW58MHx8MHx8&auto=format&fit=crop&w=500&q=60");
  background-position: center center;
  background-size: cover;
  background-repeat: no-repeat;
}

#app {
  width: 400px;
  padding: 30px;
  height: 600px;
  /* background: white; */
  background-color: rgba(255, 255, 255, 0.25);
  backdrop-filter: blur(20px);
  border-radius: 20px;
}

#app .search-box input {
  width: 100%;
  height: 40px;
  padding: 10px;
  border: none;
  outline: none;
  border-radius: 10px;
  font-size: 20px;
}

.weather-wrap .location {
  font-size: 40px;
  font-weight: 700;
  text-align: center;
  margin-top: 20px;
}
.weather-wrap .date {
  text-align: center;
}

.weather-box .temp {
  font-size: 80px;
  text-align: center;
  margin: 20px auto;
  width: 200px;
  height: 200px;
  background: white;
  line-height: 200px;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 10px;
  font-weight: bold;
}

.weather-box .weather {
  text-align: center;
  font-size: 50px;
  font-weight: 700;
}

.small{
  font-size:30px !important;

}
</style>
