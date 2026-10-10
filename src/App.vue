<template>
  <div class="weather-app">
    <header class="header">
      <div class="header-container container">
        <a class="header__logo" href="#">
          <div class="header__logo-img">
            <img src="/images/logo.svg">
          </div>
          <div class="header__logo-text">
            <h2>Weather Now</h2>
          </div>
        </a>

        <HeaderUnits/>
      </div>
    </header>

    <section class="hero container">
      <div class="hero-container">
        <h1 class="hero__title">How’s the sky looking today?</h1>

        <SearchBar 
        @search="searchCity"
        />

        <div class="hero__forecast" :class="{ hidden: isHidden }">
          <div class="hero__forecast-main">
            <div class="hero__forecast-main-now">
              <div class="hero__forecast-main-now-place">
                <span class="hero__forecast-main-now-place-location">{{ currentCity }}, {{ currenCountry }}</span>
                <span class="hero__forecast-main-now-place-date">{{ currentDate }}</span>
              </div>

              <div class="hero__forecast-main-now-info">
                <div class="hero__forecast-main-now-info-img">
                  <img :src="currentWeatherIcon">
                </div>
                <span class="hero__forecast-main-now-info-temp">{{ currentTemp }}</span>
              </div>
            </div>

            <div class="hero__forecast-main-stats">

              <MainStats
              v-for="card in mainStatsCards"
              :name="card.name"
              :value="card.value"
              />
              
            </div>

            <div class="hero__forecast-main-daily">
              <h2 class="hero__forecast-main-daily-title">
                Daily forecast
              </h2>

              <div class="hero__forecast-main-daily-grid">

                <MainDailyGrid
                v-for="card in mainDailyGridCards"
                :dayName="card.dayName"
                :icon="card.icon"
                :dayTempMax="card.dayTempMax"
                :dayTempMin="card.dayTempMin"
                />
              </div>
              
            </div>
          </div>

          <div class="hero__forecast-hourly-wrapper">
            <Hourly
            v-if="hourlyData"
            :hourly="hourlyData"
            @select-day="onDaySelected"
            />
          </div>
          
        </div>
      </div>
      
    </section>
  </div>
</template>

<script setup>
import { ref, computed }  from 'vue'
import SearchBar from './components/searchBar.vue';
import MainStats from './components/mainStats.vue';
import MainDailyGrid from './components/mainDailyGrid.vue';
import Hourly from './components/Hourly.vue';
import HeaderUnits from './components/HeaderUnits.vue';

// ------------------------------------------------------------------------
//                             Реактивные переменные  
// ------------------------------------------------------------------------
const isHidden = ref(true)
const currentCity = ref('')
const currenCountry = ref('')
const currentDate = ref('')
const currentTemp = ref('')
const currentWeatherIcon = ref('')
const currentFl = ref('')
const currentHm = ref('')
const currentWnd = ref('')
const currentPrec = ref('')

const dayName0 = ref('')
const dayName1 = ref('')
const dayName2 = ref('')
const dayName3 = ref('')
const dayName4 = ref('')
const dayName5 = ref('')
const dayName6 = ref('')

const dayIcon0 = ref('')
const dayIcon1 = ref('')
const dayIcon2 = ref('')
const dayIcon3 = ref('')
const dayIcon4 = ref('')
const dayIcon5 = ref('')
const dayIcon6 = ref('')

const dayTempMax0 = ref('')
const dayTempMin0 = ref('')
const dayTempMax1 = ref('')
const dayTempMin1 = ref('')
const dayTempMax2 = ref('')
const dayTempMin2 = ref('')
const dayTempMax3 = ref('')
const dayTempMin3 = ref('')
const dayTempMax4 = ref('')
const dayTempMin4 = ref('')
const dayTempMax5 = ref('')
const dayTempMin5 = ref('')
const dayTempMax6 = ref('')
const dayTempMin6 = ref('')

const currentdayName = ref('')
const rawHourly = ref(null)        
const selectedDayIndex = ref(0)    
const todayDayName = ref('')

const dayNames = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']

// ------------------------------------------------------------------------
// ------------------------------------------------------------------------




// ------------------------------------------------------------------------
//                             Общие методы  
// ------------------------------------------------------------------------

//Асинхронная функция для геокодинга и отправки запроса к open-meteo api
const searchCity = async (cityName) => {

  try {
    // Геокодинг
    const geoUrl = `https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(cityName)}&count=1&language=en`;
    const geoRes = await fetch(geoUrl);
    const geoData = await geoRes.json();

    if (!geoData.results || geoData.results.length === 0) {
        alert('Город не найден 😕');
        return;
    }

    const latitude = geoData.results[0].latitude;
    const longitude = geoData.results[0].longitude;
    const name = geoData.results[0].name;
    const country = geoData.results[0].country;

    // Запрос погоды по гео данным
    const weatherUrl = buildWeatherUrl(latitude, longitude)

    const weatherRes = await fetch(weatherUrl);
    const weatherData = await weatherRes.json();

    const current = weatherData.current;
    const units = weatherData.current_units;
    const daily = weatherData.daily;
    const hourly = weatherData.hourly;
    const cur_day = currentDay(weatherData.current.time)

    rawHourly.value = weatherData.hourly
    selectedDayIndex.value = 0

    const today = new Date(weatherData.current.time)
    todayDayName.value = today.toLocaleDateString('en-US', { weekday: 'long' })

    updateWeatherUi(name, country, current, units, daily, hourly)

    } catch (error) {
        console.error('Ошибка:', error);
        alert('Что-то пошло не так. Попробуйте позже.');
    }

}

//Формирование url для запроса к api
function buildWeatherUrl(latitude,longitude) {
    let url = `https://api.open-meteo.com/v1/forecast?` +
        `latitude=${latitude}&longitude=${longitude}` +
        `&current=temperature_2m,apparent_temperature,relative_humidity_2m,wind_speed_10m,precipitation,weather_code` +
        `&daily=weather_code,temperature_2m_max,temperature_2m_min` +
        `&hourly=temperature_2m,weather_code` +
        `&timezone=auto`;

    return url;
}

//Форматирование даты под дизайн
function formatDate(isoString) {
    const date = new Date(isoString);
    
    const optionsDay = { weekday: 'long' };           // День недели
    const optionsDate = { month: 'short', day: 'numeric', year: 'numeric' }; // месяц, день, год

    const dayName = date.toLocaleDateString('en-US', optionsDay);     // английский день недели
    const fullDate = date.toLocaleDateString('en-US', optionsDate);   // английская дата

    return `${dayName}, ${fullDate}`;
}

//Получение иконки по коду погоды
function getWeatherIcon(weatherCode) {
    if (weatherCode === 0) {
        return './images/icon-sunny.webp'
    }

    else if (weatherCode === 1 || weatherCode === 2) {
        return './images/icon-partly-cloudy.webp'
    }

    else if (weatherCode === 3) {
        return './images/icon-overcast.webp'
    }

    else if (weatherCode === 45 || weatherCode === 48) {
        return './images/icon-fog.webp'
    }

    else if (weatherCode >= 51 && weatherCode <= 55) {
        return './images/icon-drizzle.webp'
    }

    else if ((weatherCode >= 61 && weatherCode <= 65) || (weatherCode >= 80 && weatherCode <= 82)) {
        return './images/icon-rain.webp'
    }

    else if ((weatherCode >= 71 && weatherCode <= 75) || (weatherCode === 85 || weatherCode === 86)) {
        return './images/icon-snow.webp'
    }

    else if (weatherCode >= 95 && weatherCode <= 99) {
        return './images/icon-storm.webp'
    }

    else {
        return './images/icon-sunny.webp'
    }
}

//Форматирование времени (часы) под дизайн
function formatHour(isoString) {
  const date = new Date(isoString)
  let hours = date.getHours()

  const period = hours >= 12 ? 'PM' : 'AM'

  hours = hours % 12

  return `${hours} ${period}`
}

// Форматирование дня недели под дизайн
function formatDay(isoString) {
    const date = new Date(isoString);
    
    const optionsDay = { weekday: 'short' };           // День недели

    const dayName = date.toLocaleDateString('en-US', optionsDay);     // английский день недели

    return `${dayName}`;
}

//Получение текущего дня недели
function currentDay(isoString) {
    const date = new Date(isoString);
    
    const optionsDay = { weekday: 'long' };           // День недели

    const dayName = date.toLocaleDateString('en-US', optionsDay);     // английский день недели

    const dayNames = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];

    const currday = dayNames.indexOf(dayName)

    return currday
}

function getDayIndex(selectedDayName, todayName) {
  const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
  const todayIdx = days.indexOf(todayName)
  const selectedIdx = days.indexOf(selectedDayName)
  return (selectedIdx - todayIdx + 7) % 7
}

function getHourlyForDay(hourly, dayIndex) {
  const hours = [0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22]
  const baseIndex = dayIndex * 24

  return hours.map(hour => {
    const index = baseIndex + hour

    return {
      icon: String(getWeatherIcon(hourly.weather_code[index])),
      time: String(formatHour(hourly.time[index])),
      temp: String(Math.round(hourly.temperature_2m[index])) + '°',
    }
  })
}

function onDaySelected(dayName) {
  selectedDayIndex.value = getDayIndex(dayName, todayDayName.value)
}

//Рендер данных, полученных от api
function updateWeatherUi(name, country, current, units, daily, hourly) {

  // -------------------------------Текущая погода--------------------------------
  currentCity.value = name
  currenCountry.value = country
  currentDate.value = formatDate(current.time)
  currentTemp.value = `${Math.round(current.temperature_2m)}°`
  currentWeatherIcon.value = getWeatherIcon(current.weather_code)
  currentFl.value = `${Math.round(current.apparent_temperature)}°`
  currentHm.value = `${Math.round(current.relative_humidity_2m)} ${units.relative_humidity_2m}`
  currentWnd.value = `${Math.round(current.wind_speed_10m)} ${units.wind_speed_10m}`
  currentPrec.value = `${Math.round(current.precipitation)} ${units.precipitation}`
  currentdayName.value = currentDay(current.time)
  // -----------------------------------------------------------------------------


  // -------------------------------Погода на неделю------------------------------
  dayName0.value = formatDay(daily.time[0])
  dayName1.value = formatDay(daily.time[1])
  dayName2.value = formatDay(daily.time[2])
  dayName3.value = formatDay(daily.time[3])
  dayName4.value = formatDay(daily.time[4])
  dayName5.value = formatDay(daily.time[5])
  dayName6.value = formatDay(daily.time[6])

  dayIcon0.value = getWeatherIcon(daily.weather_code[0])
  dayIcon1.value = getWeatherIcon(daily.weather_code[1])
  dayIcon2.value = getWeatherIcon(daily.weather_code[2])
  dayIcon3.value = getWeatherIcon(daily.weather_code[3])
  dayIcon4.value = getWeatherIcon(daily.weather_code[4])
  dayIcon5.value = getWeatherIcon(daily.weather_code[5])
  dayIcon6.value = getWeatherIcon(daily.weather_code[6])

  dayTempMax0.value = `${Math.round(daily.temperature_2m_max[0])}°`
  dayTempMin0.value = `${Math.round(daily.temperature_2m_min[0])}°`
  dayTempMax1.value = `${Math.round(daily.temperature_2m_max[1])}°`
  dayTempMin1.value = `${Math.round(daily.temperature_2m_min[1])}°`
  dayTempMax2.value = `${Math.round(daily.temperature_2m_max[2])}°`
  dayTempMin2.value = `${Math.round(daily.temperature_2m_min[2])}°`
  dayTempMax3.value = `${Math.round(daily.temperature_2m_max[3])}°`
  dayTempMin3.value = `${Math.round(daily.temperature_2m_min[3])}°`
  dayTempMax4.value = `${Math.round(daily.temperature_2m_max[4])}°`
  dayTempMin4.value = `${Math.round(daily.temperature_2m_min[4])}°`
  dayTempMax5.value = `${Math.round(daily.temperature_2m_max[5])}°`
  dayTempMin5.value = `${Math.round(daily.temperature_2m_min[5])}°`
  dayTempMax6.value = `${Math.round(daily.temperature_2m_max[6])}°`
  dayTempMin6.value = `${Math.round(daily.temperature_2m_min[6])}°`
  // -----------------------------------------------------------------------------

  isHidden.value = false
}

// ------------------------------------------------------------------------
// ------------------------------------------------------------------------
// ------------------------------------------------------------------------




// ------------------------------------------------------------------------
//                        Методы для прокидывания props 
// ------------------------------------------------------------------------
const mainStatsCards = computed(() => [
  {
    name: 'Feels Like',
    value: currentFl.value
  },
  {
    name: 'Humidity',
    value: currentHm.value
  },
  {
    name: 'Wind',
    value: currentWnd.value
  },
  {
    name: 'Precipitation',
    value: currentPrec.value
  }
])

const mainDailyGridCards = computed(() => [
  {
    dayName: dayName0.value,
    icon: dayIcon0.value,
    dayTempMax: dayTempMax0.value,
    dayTempMin: dayTempMin0.value
  },
  {
    dayName: dayName1.value,
    icon: dayIcon1.value,
    dayTempMax: dayTempMax1.value,
    dayTempMin: dayTempMin1.value
  },
  {
    dayName: dayName2.value,
    icon: dayIcon2.value,
    dayTempMax: dayTempMax2.value,
    dayTempMin: dayTempMin2.value
  },
  {
    dayName: dayName3.value,
    icon: dayIcon3.value,
    dayTempMax: dayTempMax3.value,
    dayTempMin: dayTempMin3.value
  },
  {
    dayName: dayName4.value,
    icon: dayIcon4.value,
    dayTempMax: dayTempMax4.value,
    dayTempMin: dayTempMin4.value
  },
  {
    dayName: dayName5.value,
    icon: dayIcon5.value,
    dayTempMax: dayTempMax5.value,
    dayTempMin: dayTempMin5.value
  },
  {
    dayName: dayName6.value,
    icon: dayIcon6.value,
    dayTempMax: dayTempMax6.value,
    dayTempMin: dayTempMin6.value
  }
])

const hourlyData = computed(() => {
  if (!rawHourly.value) return null
  return getHourlyForDay(rawHourly.value, selectedDayIndex.value)
})
// ------------------------------------------------------------------------
// ------------------------------------------------------------------------
// ------------------------------------------------------------------------

</script>

<style lang="scss">

</style>