
<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div v-if="weather.main" class="weather-wrap">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder(new Date()) }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp) }}°C
          </div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>

    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const api = {
  key: import.meta.env.VITE_API_KEY,
  base: 'https://api.openweathermap.org/data/2.5/'
}

const query = ref('')
const weather = ref({})
function ChangeWeatherTheme(weathercondition) {
  if (weathercondition === 'Clear') {
    backgroundcolor = document.body.style.background = "linear-gradient(to right, #2980b9, #6dd5fa, #ffffff)";
  } else if (weathercondition === 'Clouds') {
    document.body.style.background = "linear-gradient(to right, #d7d2cc, #304352)";
  } else if (weathercondition === 'Rain') {
    document.body.style.background = "linear-gradient(to right, #4e54c8, #8f94fb)";
  } else if (weathercondition === 'Snow') {
    document.body.style.background = "linear-gradient(to right, #83a4d4, #b6fbff)";
  } else {whatercondition === 'stormy'
    document.body.style.background = "linear-gradient(to right, #83a4d4, #b6fbff)";
  }
  
}


function fetchWeather(e) {
  if (e.key === 'Enter') {
    fetch(`${api.base}weather?q=${query.value}&units=metric&APPID=${api.key}`)
      .then(res => res.json())
      .then(result => {
        weather.value = result
        query.value = ''
        console.log(result)
      })
  }
}


function dateBuilder(d) {
  const months = [
    'January', 'February', 'March', 'April', 'May', 'June',
    'July', 'August', 'September', 'October', 'November', 'December'
  ]
  const days = [
    'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'
  ]

  const day = days[d.getDay()]
  const date = d.getDate()
  const month = months[d.getMonth()]
  const year = d.getFullYear()

  return `${day} ${date} ${month} ${year}`
}
</script>

<style>
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
  background: linear-gradient(to right, #83a4d4, #b6fbff);
  color: #333;
  text-align: center;
}

main {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.search-box {
  margin-bottom: 2rem;
}

.search-bar {
  padding: 10px;
  border-radius: 8px;
  border: none;
  width: 250px;
  outline: none;
}

.weather-box .temp {
  font-size: 48px;
  font-weight: bold;
}
</style>