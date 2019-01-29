<template>
  <div class="weather">
    <h1>Current weather App built with VueJS and OpenWeatherMap API</h1>
    <div class="container">
      <h2 class="text-big">{{ temp }}<i class="wi wi-celsius"></i></h2>
      <i class="wi wi-cloudy-windy" id="big-icon"></i>
      <span id="description">{{ description }}</span>
      <p>{{ locationName }}</p>
      <div class="flex">
        <div class="flex-item">
          <i class="wi wi-windy block"></i>
          <i class="wi wi-humidity block"></i>
          <i class="wi wi-barometer block"></i>  
        </div>
        <div class="flex-item">
          <p class="info"> {{ wind }} m/s</p>
          <p class="info"> {{ humid }}%</p>
          <p class="info"> {{ pressure }} Atm</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Weather',
    data() {
      return {
        locationName: '',
        temp: '',
        humid: '',
        wind: '',
        description: '',
        pressure: '',
      }
    },
    created() {
      if(navigator.geolocation){
        navigator.geolocation.getCurrentPosition(pos => {
          // eslint-disable-next-line
          console.log('Pos coordinates', pos.coords)
          let lat = pos.coords.latitude
          let lng = pos.coords.longitude
          this.getWeather(lat, lng)
        })
      } else {
          // eslint-disable-next-line
        console.log('Geolocation is not enabled in your browser')
      }
    },
    methods: {
      getWeather(lat, lng) {
          let url = `http://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lng}&APPID=d212cdc6d9f0c0909eb57551783e4ab9`

          axios.get(url)
            .then(res => {
            // eslint-disable-next-line
              console.log(res)
              this.locationName = res.data.name,
              this.temp = Math.floor(res.data.main.temp - 273.15),
              this.humid = res.data.main.humidity,
              this.pressure = res.data.main.pressure,
              this.description = res.data.weather[0].description,
              this.wind = res.data.wind.speed
            })
            // eslint-disable-next-line
            .catch(err => console.log(err))

        }

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.weather {
  margin: 10px;
}

.container {
  margin: 10px;
  padding: 35px 0;
  border-radius: 15%;
  box-shadow: 1px 1px 10px black;
}

#big-icon {
  font-size: 30px;
}

#description {
  margin: 0 5px;
  text-transform: capitalize;
}

.text-big {
  font-size: 70px;
  margin: 0;
}

.flex {
  display: flex;
  justify-content: center;
}

.flex-item {
  margin: 0 5px;
}

.block {
  display: block;
  margin: 5px 0;
}

.info {
  margin: 5px 0;
  text-align: left;
}
</style>
