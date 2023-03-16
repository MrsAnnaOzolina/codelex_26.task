<script setup lang="ts">

import axios from 'axios'
import { nextTick, onBeforeUpdate, onMounted, onServerPrefetch, onUpdated, ref, watch } from 'vue'

const url = "http://api.weatherapi.com/v1/current.json?key=fe37194fc13c40978da103109231503&aqi=yes&q="

let  value = "Dobele"
const data = ref({})
const data1 = ref({})
const data2 = ref({})
const co = ref(0)
const pm2_5 = ref(0)
const pm10 = ref(0)
let image = ""
let imageText = ""
const toggle = ref(false)
const props = defineProps(['value'])
let loadingState =  true



async function getData(value: string) {
  loadingState = false
   axios.get(url + value)
    .then((response) => {
      data.value = response.data.location
      data1.value = response.data.current
      data2.value = response.data.current.air_quality
      imageText = response.data.current.condition.text
      image = response.data.current.condition.icon
      co.value = response.data.current.air_quality.co.toFixed(2)
      pm2_5.value = response.data.current.air_quality.pm2_5.toFixed(2)
      pm10.value = response.data.current.air_quality.pm10.toFixed(2)
      console.log(response)
      loadingState = true
      console.log(loadingState)
    })
    .catch((error) => {
      console.log(error)
    })

  }


onMounted(() => {
   getData(value);
   
}),

watch(props, ()=>
    getData(props.value) 
  );


</script>

<template>
<div v-if="loadingState">
  <div class="toogle">
    <span>Show {{ !toggle ? "°F" : "°C" }}</span>
    <label class="switch">
      <input type="checkbox" v-model="toggle" value=false unchecked-value=true>
      <span class="slider round"></span>
    </label>
  </div>
  <span class="card__logo">
    <svg width="150" height="150" viewBox="0 0 176 176" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path
        d="M155.809 80.6055C155.755 80.6055 155.704 80.6211 155.65 80.6211C156.651 78.3136 157.211 75.7635 157.211 73.0804C157.211 62.5672 148.688 54.0409 138.172 54.0409C134.456 54.0409 131 55.1204 128.069 56.9664C124.106 47.3293 114.635 40.5396 103.572 40.5396C88.9422 40.5396 77.082 52.3982 77.082 67.0338C77.082 68.1367 77.1712 69.2162 77.3026 70.2878C77.2283 70.2878 77.1563 70.28 77.082 70.28C67.2728 70.28 59.3206 78.2275 59.3206 88.0367C59.3206 97.8459 67.2728 105.801 77.082 105.801H155.809C162.766 105.801 168.405 100.161 168.405 93.2072C168.405 86.2453 162.766 80.6055 155.809 80.6055Z"
        fill="#C2CEF2" />
      <path
        d="M124.246 97.1804C124.18 97.1804 124.12 97.1961 124.054 97.1961C125.261 94.4035 125.937 91.3294 125.937 88.0987C125.937 75.4188 115.659 65.148 102.982 65.148C98.5012 65.148 94.3342 66.4465 90.8008 68.6681C86.0214 57.0519 74.604 48.862 61.2638 48.862C43.6245 48.862 29.3245 63.169 29.3245 80.8083C29.3245 82.1381 29.4324 83.4445 29.5912 84.7352C29.5013 84.7352 29.4145 84.7195 29.3245 84.7195C17.498 84.7195 7.91013 94.3097 7.91013 106.136C7.91013 117.963 17.498 127.551 29.3245 127.551H124.246C132.634 127.551 139.434 120.751 139.434 112.364C139.434 103.976 132.634 97.1804 124.246 97.1804Z"
        fill="white" />
    </svg>
  </span>
  <div class="uk-card uk-card-hover uk-card-secondary .uk-align-center card" >
    <div class="card__first">
      <div class="card__box">
        <p class="card--text"><svg width="20px" fill="#b0a6a6" version="1.1" id="Capa_1"
            xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 395.71 395.71"
            xml:space="preserve">
            <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
            <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
            <g id="SVGRepo_iconCarrier">
              <g>
                <path
                  d="M197.849,0C122.131,0,60.531,61.609,60.531,137.329c0,72.887,124.591,243.177,129.896,250.388l4.951,6.738 c0.579,0.792,1.501,1.255,2.471,1.255c0.985,0,1.901-0.463,2.486-1.255l4.948-6.738c5.308-7.211,129.896-177.501,129.896-250.388 C335.179,61.609,273.569,0,197.849,0z M197.849,88.138c27.13,0,49.191,22.062,49.191,49.191c0,27.115-22.062,49.191-49.191,49.191 c-27.114,0-49.191-22.076-49.191-49.191C148.658,110.2,170.734,88.138,197.849,88.138z">
                </path>
              </g>
            </g>
          </svg>{{ data.country }}, {{ data.name }}</p>
        <h1 class="card--title"> {{ toggle ? data1.temp_f : data1.temp_c }}
          <span class="card--article"> {{ toggle ? "°F" : "°C" }} </span>
        </h1>
        <p class="card--subtitle">Feels like {{ toggle ? data1.feelslike_f : data1.feelslike_c }} {{ toggle ? "°F" : "°C"
        }} </p>
      </div>
    </div>
    <div class="card__second">
      <div class="card__box">
        <h5 class="card--subtitle">
          <svg width="20" height="19" viewBox="0 0 20 19" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M15 5C6 7 3.9 13.17 1.82 18.34L3.71 19L4.66 16.7C5.14 16.87 5.64 17 6 17C17 17 20 0 20 0C19 2 12 2.25 7 3.25C2 4.25 0 8.5 0 10.5C0 12.5 1.75 14.25 1.75 14.25C5 5 15 5 15 5Z"
              fill="#DAD8F7" />
          </svg>
          Air quality
        </h5>
        <div class="card--airQualoty">
          <p class="card--subtitle card--textColor">{{ co }}</p>
          <p class="card--subtitle card--textColor">{{ pm2_5 }}</p>
          <p class="card--subtitle card--textColor">{{ pm10 }}</p>
          <p class="card--subtitle">CO</p>
          <p class="card--subtitle">PM2.5</p>
          <p class="card--subtitle">PM10 </p>
        </div>
      </div>
    </div>
    <div class="card__third">
      <div class="card__box">
        <div class="card--subtitle">
          <svg width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
            <g opacity="0.4">
              <path
                d="M3.45539 7.66197C3.45539 6.94084 3.93614 6.46009 4.65727 6.46009H5.85914C6.58027 6.46009 7.06102 6.94084 7.06102 7.66197C7.06102 8.3831 6.58027 8.86385 5.85914 8.86385H4.65727C3.93614 8.86385 3.45539 8.3831 3.45539 7.66197ZM4.65727 17.277C4.65727 16.5559 5.13802 16.0751 5.85914 16.0751H7.06102C7.78215 16.0751 8.2629 16.5559 8.2629 17.277C8.2629 17.9981 7.78215 18.4789 7.06102 18.4789H5.85914C5.13802 18.4789 4.65727 17.9981 4.65727 17.277ZM4.65727 20.8826C3.93614 20.8826 3.45539 21.3634 3.45539 22.0845C3.45539 22.8056 3.93614 23.2864 4.65727 23.2864H14.2723C14.9934 23.2864 15.4742 22.8056 15.4742 22.0845C15.4742 21.3634 14.9934 20.8826 14.2723 20.8826H4.65727ZM25.0892 7.66197C25.0892 6.94084 25.5699 6.46009 26.2911 6.46009H28.6948C29.416 6.46009 29.8967 6.94084 29.8967 7.66197C29.8967 8.3831 29.416 8.86385 28.6948 8.86385H26.2911C25.5699 8.86385 25.0892 8.3831 25.0892 7.66197ZM8.2629 25.6901C7.54177 25.6901 7.06102 26.1709 7.06102 26.892C7.06102 27.6131 7.54177 28.0939 8.2629 28.0939H16.676C17.3972 28.0939 17.8779 27.6131 17.8779 26.892C17.8779 26.1709 17.3972 25.6901 16.676 25.6901H8.2629ZM3.45539 13.6714C2.73426 13.6714 2.25351 13.1906 2.25351 12.4695C2.25351 11.7484 2.73426 11.2676 3.45539 11.2676H17.8779C19.2 11.2676 20.2817 10.1859 20.2817 8.86385C20.2817 7.54178 19.2 6.46009 17.8779 6.46009C17.1568 6.46009 16.5559 6.70047 16.1953 7.18122C15.7145 7.66197 14.9934 7.66197 14.5127 7.18122C14.0319 6.70047 14.0319 5.97934 14.5127 5.49859C15.354 4.65727 16.5559 4.05634 17.8779 4.05634C20.5221 4.05634 22.6854 6.21972 22.6854 8.86385C22.6854 11.508 20.5221 13.6714 17.8779 13.6714H3.45539ZM26.2911 16.0751C27.0122 16.0751 27.4929 15.5944 27.4929 14.8732C27.4929 14.1521 27.0122 13.6714 26.2911 13.6714C25.9305 13.6714 25.6901 13.7915 25.4498 14.0319C24.969 14.5127 24.2479 14.5127 23.7671 14.0319C23.2864 13.5512 23.2864 12.83 23.7671 12.3493C24.3681 11.6282 25.3296 11.2676 26.2911 11.2676C28.3343 11.2676 29.8967 12.83 29.8967 14.8732C29.8967 16.9164 28.3343 18.4789 26.2911 18.4789H15.4742C14.753 18.4789 14.2723 17.9981 14.2723 17.277C14.2723 16.5559 14.753 16.0751 15.4742 16.0751H26.2911ZM25.0892 23.2864H19.0798C18.3587 23.2864 17.8779 22.8056 17.8779 22.0845C17.8779 21.3634 18.3587 20.8826 19.0798 20.8826H25.0892C27.1324 20.8826 28.6948 22.4451 28.6948 24.4883C28.6948 26.5315 27.1324 28.0939 25.0892 28.0939C24.1277 28.0939 23.1662 27.7333 22.5652 27.0122C22.0845 26.5315 22.0845 25.8103 22.5652 25.3296C23.046 24.8488 23.7671 24.8488 24.2479 25.3296C24.4883 25.57 24.7286 25.6901 25.0892 25.6901C25.8103 25.6901 26.2911 25.2094 26.2911 24.4883C26.2911 23.7671 25.8103 23.2864 25.0892 23.2864Z"
                fill="white" />
            </g>
          </svg>
          <h5>Wind</h5>
        </div>
        <h4 class="card--wind"> {{ data1.wind_kph }} <span class="card--article2"> km/h</span> </h4>
      </div>
    </div>
    <div class="card__fourth">
      <div class="card--subtitle">
        <h1>{{ imageText }}</h1>
        <img :src="image" alt="logo">
      </div>
    </div>
  </div>
</div>
</template>

<style scoped>
.card__logo {
  position: absolute;
  z-index: 1;
}

@media (max-width: 900px) {
  .card {
    /* padding: 30px; */
    display: grid;
    justify-content: space-around;
    grid-template-columns: repeat(4, 100px);
    gap: 30px;
    color: white;
    margin: 20px;
  }

  .card__image {
    width: 20px;
  }

  .card__first {
    background: linear-gradient(to top, #6660C8 0%, #7770f1 100%);
    border-radius: 10px;
    grid-column: 1/5;
    grid-row: 1;
    display: grid;
    grid-template-columns: repeat(1, 80vw);
    backdrop-filter: blur(10px);
  }

  .card__second {
    background: linear-gradient(to top, #6660C8 0%, #7770f1 100%);
    border-radius: 10px;
    grid-column: 1/3;
    grid-row: 2;
  }

  .card__third {
    background: linear-gradient(to top, #6660C8 0%, #7770f1 100%);
    border-radius: 10px;
    grid-column: 3/5;
    grid-row: 2;
  }

  .card__fourth {
    background: linear-gradient(to top, #6660C8 0%, #7770f1 100%);
    border-radius: 10px;
    grid-column: 1/5;
    grid-row: 3;
  }
}

@media (min-width: 901px) {
  .card {
    padding: 30px;
    display: grid;
    justify-content: space-around;
    grid-template-columns: repeat(3, 30vw);
    gap: 30px;
    color: white;
    margin: 20px;
  }

  .card__first {
    background: linear-gradient(to top, #6660C8 0%, #7770f1 100%);
    border-radius: 10px;
    grid-column: 1;
    grid-row: 1 / 3;
  }

  .card__second {
    background: linear-gradient(to top, #6660C8 0%, #7770f1 100%);
    border-radius: 10px;
  }

  .card__third {
    background: linear-gradient(to top, #6660C8 0%, #7770f1 100%);
    border-radius: 10px;
  }

  .card__fourth {
    background: linear-gradient(to top, #6660C8 0%, #7770f1 100%);
    border-radius: 10px;
    grid-column: 2/4;
    grid-row: 2;
  }
}

.card--wind {
  display: flex;
  justify-content: center;
  align-items: flex-end;
}

.card__box {
  margin-top: 20px;
  margin-bottom: 20px;
  margin-right: 20px;
}

.card--text {
  color: #C2BFF4;
  display: flex;
  justify-content: flex-end;
}

.card--title {
  display: flex;
  justify-content: center;
  font-size: 6em;
  margin: 0;
}

.card--article {
  font-size: 0.4em;
  color: #DAD8F7;
}

.card--article2 {
  font-size: 0.8em;
  color: #DAD8F7;
  margin-left: 3px;
}

.card--subtitle {
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card--textColor {
  color: #87EBCD;
  margin-bottom: 5px;
}

.card--airQualoty {
  display: grid;
  grid-template-columns: repeat(3, 10vw);
  font-size: 10px;
  justify-content: center;
  margin-top: 30px;
}

.toogle {
  font-size: 20px;
  color: white;
  display: grid;
  justify-content: flex-end;
  grid-template-columns: repeat(2, 85px);
  margin-right: 20px;
}

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
  margin-left: 10px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgb(189, 177, 177);
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: rgb(255, 255, 255);
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked+.slider {
  background-color: rgb(120, 117, 117);
}

input:focus+.slider {
  box-shadow: 0 0 1px darkgrey;
}

input:checked+.slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}</style>
