<template>
<div>
  <div class="container-fluid bg-dark text-white">
  <div class="card-header d-flex main">
    <h5>Devayo Weather App</h5>
    <p class="ms-auto">Welcome To Devayo Weather App</p>
  </div>
</div>
<div class="mains">
  <div class="container">
  <div class="row">
    <div class="col-md-4 mt-4 ms-auto">
      <div class="card-header">
        Search Section
      </div>
      <div class="card-body border">
      <form v-on:submit.prevent="getWeather">
         <div class="form-group">
            <input type="text" placeholder="What City?" class="form-control" v-model="citySearch" autocomplete="off"> 
            <button class="btn btn-success mt-3" type="submit">Submit</button>
        </div>
      </form>
      </div>
    </div>

     <div class="col-md-8 mt-4">
      <div class="card-header">
        Display Section
      </div>
      <div class="card-body border">
        <h4 class="text-center">{{weather.cityName}}</h4>
        <span><h5 class="text-center">{{weather.country}}</h5></span>
      <div class="text-center">
         <span><h2 class="mt-5">{{weather.temperature}}°C</h2></span>
         <small>{{weather.description}}</small>
      </div>
      <div class="d-flex container justify-content-end">
        <p class="ml-auto">{{weather.lowTemp}}°C</p>
        <p class="ms-auto">{{weather.highTemp}}°C</p>
      </div>

      <div class="d-flex container justify-content-end mt-4">
       <div class="ml-auto">
          <h3>{{weather.feelsLike}}°C</h3>
          <small>Feels Like</small>
       </div>
       <div class="ms-auto">
          <h3>{{weather.humidity}}°C</h3>
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
  data(){
    return{
      citySearch: "",
      weather: {
        cityName: "Abuja",
        country: "NG",
        temperature: 12,
        description: "Cloud everywhere",
        lowTemp: "19",
        highTemp: "24",
        feelsLike: "20",
        humidity: "55"

      }
    };
  },
  methods: {
    getWeather: async function(){
      console.log(this.citySearch);
      const key = "87aee6a845d121670d18ea1a1c036571";
      const baseURL = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`;
      this.citySearch = "";
    },
  },
}
</script>

<style>
  .mains{
    background-color: aliceblue;
    height: calc(100vh - 58px);
    width: 100vw;
  }
  @media screen and (max-width: 539px) {
    .main p{
      display: none;
    }
}
</style>
