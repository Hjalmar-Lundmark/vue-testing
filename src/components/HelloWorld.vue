<script setup>
import { ref, watch } from 'vue'

defineProps({
  msg: String,
})

const count = ref(0)
const weatherData = ref(null)
const lat = ref(0)
const long = ref(0)

watch(
  count,
  async function fetchStuff() {
    lat.value = Math.random() * 180 - 90
    long.value = Math.random() * 360 - 180

    await fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${lat.value}&lon=${long.value}&appid=${import.meta.env.VITE_API_KEY}`)
      .then(res => res.json())
      .then(result => {
        console.log(result)
        weatherData.value = result
      }).catch(err => {
        console.log(err)
      });

    console.log(weatherData._rawValue)
  }
)

function getWeatherIcon(weather) {
  return 'http://openweathermap.org/img/w/' + weather + '.png'
}

</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <!-- <button type="button" @click="fetchStuff">Fetch</button> -->
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank">create-vue</a>, the official Vue + Vite
    starter
  </p>
  <p>
    Install
    <a href="https://github.com/vuejs/language-tools" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>

  <div class="weather" v-if="weatherData">
    <div className='gridCard'>
      <h3>City:</h3>
      <h2>{{ weatherData.name }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Temperature:</h3>
      <h2>{{ (weatherData.main.temp - 273.15).toFixed(1) + ' °C' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Feels like:</h3>
      <h2>{{ (weatherData.main.feels_like - 273.15).toFixed(1) + ' °C' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Humidity:</h3>
      <h2>{{ weatherData.main.humidity + '%' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Pressure:</h3>
      <h2>{{ weatherData.main.pressure + ' hPa' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Wind speed:</h3>
      <h2>{{ weatherData.wind.speed + ' m/s' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Wind direction:</h3>
      <h2>{{ weatherData.wind.deg + ' °' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Cloudiness:</h3>
      <h2>{{ weatherData.clouds.all + '%' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Visibility:</h3>
      <h2>{{ weatherData.visibility + ' m' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Weather:</h3>
      <h2>{{ weatherData.weather[0].main }}</h2>
    </div>
    <!-- <div className='gridCard' v-if="weatherData.weather[0].icon">
      <h3>Weather icon:</h3>
      <img src={{ getWeatherIcon(weatherData.weather[0].icon) }} alt={{weatherData.weather[0].description}} width={{100}}
        height={{100}} />
    </div> -->
    <div className='gridCard'>
      <h3>Base:</h3>
      <h2>{{ weatherData.base }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Timezone:</h3>
      <h2>{{ weatherData.timezone + ' s' }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Country:</h3>
      <h2>{{ weatherData.sys.country }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Sunrise:</h3>
      <h2>{{ new Date(weatherData.sys.sunrise * 1000).toLocaleTimeString() }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Sunset:</h3>
      <h2>{{ new Date(weatherData.sys.sunset * 1000).toLocaleTimeString() }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Coordinates:</h3>
      <h2>{{ weatherData.coord.lat + ', ' + weatherData.coord.lon }}</h2>
    </div>



  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
