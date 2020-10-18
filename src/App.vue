<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div class="container my-5">
      <h1 class="title text-center">Weather in</h1>
      <form class="search-location" v-on:submit.prevent="getWeather">
      <input type="submit" class="form-rounded form-control text-light my-3 shadow-lg back-card overflow-hidden" value="My Region">
      </form>

      <div
        class="card rounded my-3 shadow-lg back-card overflow-hidden" v-if="visible">
        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-3">
            <p>{{weather.cityName}}</p>
            <p class="">{{weather.country}}</p>
            <div>
             <div class="icon sunny" v-if="clearSky">
               <div class="sun">
               <div class="rays"></div>
               </div>
             </div>
             <div class="icon flurries" v-if="snowy">
               <div class="cloud"></div>
               <div class="snow">
               <div class="flake"></div>
               <div class="flake"></div>
               </div>
             </div>
             <div class="icon rainy" v-if="stormy">
               <div class="cloud"></div>
               <div class="rain"></div>
             </div>
             <div class="icon cloudy" v-if="cloudy">
               <div class="cloud"></div>
               <div class="cloud"></div>
             </div>
            </div>
          </div>
        </div>

        <div class="card-body">
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <span>{{weather.temperature}}&deg;C</span>
                <p class="my-4">{{weather.description}}</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  <img src="./assets/down.svg" alt="" />
                  {{weather.lowTemp}}&deg;C
                </p>
                <p>
                  <img src="./assets/up.svg" alt="" />
                  {{weather.highTemp}}&deg;C
                </p>
              </div>
            </div>
          </div>
          <div class="card-bottom px-5 py-4 row">
            <div class="col text-center">
              <p>{{weather.feelsLike}}&deg;C</p>
              <span>Feels like</span>
            </div>
            <div class="col text-center">
              <p>{{weather.humidity}}%</p>
              <span>humidity</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
    isDay: true,
    stormy: false,
    cloudy: false,
    clearSky: false,
    snowy: false,
    visible: false,
    weather: {
        cityName: "Ulyanovsk",
        country: "Russia",
        temperature: 12,
        description: "Clouds everywhere",
        lowTemp: "9",
        highTemp: "16",
        feelsLike: "10",
        humidity: "5",
      },
    };
  },

methods: {
  getWeather: function(){
    console.log("ghj");
    const key = "5299e83c9cf2fef510fb8ddaa3206ac8";
    const baseURL = `http://api.openweathermap.org/data/2.5/weather?q=Ulyanovsk,Russia&appid=${key}&units=metric`;

    fetch(baseURL)
  .then((response) => {
    return response.json();
  })
  .then((data) => {
    this.weather.cityName = data.name;
    this.weather.country = data.sys.country;
    this.weather.temperature = Math.round(data.main.temp);
    this.weather.description = data.weather[0].description;
    this.weather.lowTemp = Math.round(data.main.temp_min);
    this.weather.highTemp = Math.round(data.main.temp_max);
    this.weather.feelsLike = Math.round(data.main.feels_like);
    this.weather.humidity = Math.round(data.main.humidity);
    const timeOfDay = data.weather[0].icon;
        if (timeOfDay.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }
    const mainWeather = data.weather[0].main;
    if (mainWeather.includes("Clouds")) {
      this.stormy = false;
      this.cloudy = true;
      this.clearSky = false;
      this.snowy = false;
    }
    if (mainWeather.includes("Clouds")) {
      this.stormy = false;
      this.cloudy = true;
      this.clearSky = false;
      this.snowy = false;
    }
    if (
      mainWeather.includes("Thunderstorm") ||
      mainWeather.includes("Rain")
    ) {
      this.stormy = true;
      this.cloudy = false;
      this.clearSky = false;
      this.snowy = false;
    }
    if (mainWeather.includes("Clear") && this.isDay) {
      this.stormy = false;
      this.cloudy = false;
      this.clearSky = true;
      this.snowy = false;
    }
    if (mainWeather.includes("Clouds") && !this.isDay) {
      this.stormy = false;
      this.cloudy = false;
      this.clearSky = false;
      this.snowy = false;
    }
    if (mainWeather.includes("Snow")) {
      this.stormy = false;
      this.cloudy = false;
      this.clearSky = false;
      this.snowy = true;
    }
    this.visible = true;
  });
  },
}
};
</script>

<style>
  @import "./assets/animation.css";
  @import "./assets/custom.css";
</style>
