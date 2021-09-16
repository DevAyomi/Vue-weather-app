<template>
  <div>
    <div class="container-fluid bg-dark text-white">
      <div class="card-header d-flex main">
        <h5>Devayo Weather App</h5>
        <p class="ms-auto">Welcome To Devayo Weather App</p>
      </div>
    </div>
    <div class="mains" :class="isDay ? 'light-theme' : 'dark-theme'">
      <div class="container">
        <div class="row">
          <div class="col-md-4 mt-4 ms-auto">
            <p v-if="danger" class="justify-content-center p-2 bg-warning card-header">City not found</p>
             <p v-if="!danger" class="card-header">Search any city around the world</p>
            <div class="card-body border">
              <form v-on:submit.prevent="getWeather">
                <div class="form-group">
                  <input
                    type="text"
                    placeholder="What City?"
                    class="form-control"
                    v-model="citySearch"
                    autocomplete="off"
                  />
                  <button class="btn btn-success mt-3" type="submit">
                    Submit
                  </button>
                </div>
              </form>
            </div>
          </div>

          <div class="col-md-8 mt-4">
            <div class="card-header">
              <div class="d-flex">
                <p>Display Section</p>
                <h5 v-if="isDay" class="ms-auto">Day-Time</h5>
                <h5 v-else class="ms-auto">Night</h5>
              </div>
            </div>
            <div class="card-body border">
              <h4 class="text-center">{{ weather.cityName }}</h4>
              <span
                ><h5 class="text-center">{{ weather.country }}</h5></span
              >
              <div class="text-center">
                <span
                  ><h2 class="mt-5">{{ weather.temperature }}°C</h2></span
                >
                <small>{{ weather.description }}</small>
              </div>
              <div class="d-flex container justify-content-end">
                <p class="ml-auto">{{ weather.lowTemp }}°C</p>
                <p class="ms-auto">{{ weather.highTemp }}°C</p>
              </div>

              <div class="d-flex container justify-content-end mt-4">
                <div class="ml-auto">
                  <h3>{{ weather.feelsLike }}°C</h3>
                  <small>Feels Like</small>
                </div>
                <div class="ms-auto">
                  <h3>{{ weather.humidity }}°C</h3>
                  <small>Humidity</small>
                </div>
              </div>
            </div>
            <div class="card-footer">
              <p>Application by Devayo</p>
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
      danger: false,
      citySearch: "",
      weather: {
        cityName: "Placeholder",
        country: "Ng",
        temperature: "XX",
        description: "Placeholder",
        lowTemp: "XX",
        highTemp: "XX",
        feelsLike: "XX",
        humidity: "XX",
      },
    };
  },
  methods: {
    getWeather: async function () {
      console.log(this.citySearch);
      const key = "87aee6a845d121670d18ea1a1c036571";
      const baseURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;
      const response = await fetch(baseURL);
      if (response.status >= 200 && response.status <= 299) {
        const data = await response.json();
        console.log(data);
        this.citySearch = "";
        this.weather.cityName = data.name;
        this.weather.country = data.sys.country;
        this.weather.temperature = Math.round(data.main.temp);
        this.weather.description = data.weather[0].description;
        this.weather.lowTemp = Math.round(data.main.temp_min);
        this.weather.highTemp = Math.round(data.main.temp_max);
        this.weather.feelsLike = Math.round(data.main.feels_like);
        this.weather.humidity = Math.round(data.main.humidity);
        this.danger = false;
        const DayNight = data.weather[0].icon;

        if (DayNight.includes("n")) {
          this.isDay = false;
        } else {
          this.isDay = true;
        }
      }else{
        //Check the time of the day
         if(response.statusText == "Not Found"){
          this.danger = "true"
         }else{
           this.danger = "false"
         }
      }
      
    },
  },
};
</script>

<style>
.mains {
  height: calc(100vh - 58px);
  width: 100vw;
}

.light-theme {
  background: linear-gradient(to bottom left, #d7d3ac, #ffffff);
}

.dark-theme {
  background: linear-gradient(to bottom left, #4854a2, #3d3d3d);
  color: white;
}
@media screen and (max-width: 539px) {
  .main p {
    display: none;
  }
}
</style>
