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

    console.log(weatherData.value)
  }
)

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
      <h2>{{ weatherData.value.name }}</h2>
    </div>
    <div className='gridCard'>
      <h3>Temperature:</h3>
      <h2>{{ (weatherData.value.main.temp - 273.15).toFixed(1) + ' °C' }}</h2>
    </div>
    <!-- <div className='gridCard'>
      <h3>Feels like:</h3>
      <h2>{{ (weatherData.value.main.feels_like - 273.15).toFixed(1) + ' °C' }}</h2>
    </div>
    {{ weatherData.value.rain ?
      (
        <div className= 'gridCard' >
          <h3>Rainfall : </h3>
          < h2 > {{
            weatherData.value.rain["1h"] }} mm/h</h2>
          </div>
          ) : <></>}}
          {{ weatherData.value.snow ?
            (
              <div className= 'gridCard' >
                <h3>Snowfall : </h3>
                < h2 > {{
                  weatherData.value.snow["1h"] }} mm/h</h2>
                </div>
                ) : <></>}} 
                <div className='gridCard'>
                  <h3>Clouds:</h3>
                  <h2>Sky coverage: {{ weatherData.value.clouds.all + '%' }}</h2>
                </div>
                <div className='gridCard'>
                  <h3>Pressure:</h3>
                  <h2>{{ weatherData.value.main.pressure }} hPa</h2>
                </div>
                <div className='gridCard'>
    <h3>Wind:</h3>
    <h2>
      {{ getWind(weatherData.value) }} ({{ weatherData.value.wind.speed + ' m/s' }})
    </h2>
  </div>
  <div className='gridCard'>
    <h3>Wind direction:</h3>
    <h2>
      {{ getWindDir(weatherData.value) }} ({{ weatherData.value.wind.deg + '°' }})
    </h2>
  </div>
  <div className='gridCard'>
    <h3>Humidity:</h3>
    <h2>{{ weatherData.value.main.humidity + '%' }}</h2>
  </div>
  <div className='gridCard'>
    <h3>Visibility:</h3>
    <h2>{{ weatherData.value.visibility }} meter</h2>
  </div>
  <div className='gridCard'>
    <h3>Sunrise:</h3>
    <h2>{{ new Date(weatherData.value.sys.sunrise * 1000).toLocaleTimeString() }}</h2>
  </div>
  <div className='gridCard'>
    <h3>Sunset:</h3>
    <h2>{{ new Date(weatherData.value.sys.sunset * 1000).toLocaleTimeString() }}</h2>
  </div> -->
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
