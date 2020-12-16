<template>
  <div id="app"
  :class="typeof data.current != 'undefined' && data.current.temp > 19 ? 'warm' : ''"
  >
    <div class="weather-wrap">
      <div class="weather-head">
        <div class="title">
          <h1>Weather-App</h1>
        </div>
      </div>
      <div class="weather-box">
        <div class="local-wrap">
          <div class="time-zone">{{ data.timezone }}</div>
          <div class="local-date">{{ setDate() }}</div>
          <div class="local-temp">{{ Math.round(data.current.temp) }}&deg;C</div>
          <div class="desc">{{ data.current.weather[0].description }}</div>
          <div class="icon"><img :src="`http://openweathermap.org/img/wn/${data.current.weather[0].icon}@2x.png`"/></div>
        </div>
      </div>
      <div class="forecast-wrap">
          <h1>8 Day Forecast</h1>
        <div class="forecast-box">
          <ul class="forecast-items" v-for="item in this.data.daily" :key="item">
            <li class="forecast-avg">Average: {{ Math.round(item.temp.day, 2) }}&deg;C</li>
            <li class="forecast-max">Max: {{ Math.round(item.temp.max, 2)}}&deg;C</li>
            <li class="forecast-min">Min: {{ Math.round(item.temp.min, 2)}}&deg;C</li>
            <li class="forecast-desc">Description: {{ item.weather[0].main }}</li>
            <li class="forecast-icon"><img :src="`http://openweathermap.org/img/wn/${ item.weather[0].icon }@2x.png`" /></li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      data: {},
      url: 'https://api.openweathermap.org/data',
      key: '75894b3d332255c6a0805b8994b67a7d',
      day: ["Sun", "Mon", "Tue", "Wed", "Thurs", "Fri", "Sat"]
    }
  },
  created() {
    if(navigator.geolocation){
      navigator.geolocation.getCurrentPosition(pos => {
        let lat = pos.coords.latitude;
        let lon = pos.coords.longitude;
        //console.log("lat: " + lat + ", " + "long: " + lon);

        fetch(`${this.url}/2.5/onecall?lat=${lat}&lon=${lon}&units=metric&appid=${this.key}`)
          .then(res => {
            return res.json()
          }).then(this.setData);
      })
    }
  },
  methods: {
    setData(results){
      this.data = results
    },

    setDate(){
      let d = new Date();
      let months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sept", "Oct", "Nov", "Dec"]
      let days = ["Sun", "Mon", "Tue", "Wed", "Thurs", "Fri", "Sat"]

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`;
    },
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #dee7f0;
  text-shadow: -2px 2px 2px 3px #0000;
  margin: 0;
  background-image: url('./assets/coldbg.jpg');
  background-size: cover;
  width: 100vw;
  height: 100vh;
  align-content: center;
}

#app.warm {
  background-image: url('./assets/warmbg.jpg');
  background-size: cover;
}

.weather-wrap {
  position: relative;
  margin-left: auto;
  margin-right: auto;
}

.weather-box {
  background-color: rgba(0, 0, 0, 0.4);
  position: relative;
  width: 20%;
  height: 20%;
  padding: 1.4%;
  border-radius: 5px;
  margin-left: auto;
  margin-right: auto;
  font-size: 18px;
}

.forecast-wrap {
  margin-top: 60px;
  width: 90vw;
  height: auto;
  background-color: rgba(0, 0, 0, 0.4);
  margin: 50px;
  padding: 30px;
}

.forecast-box {
  display: flex;
  flex-wrap: nowrap;
  justify-content: center;
}

.forecast-items {
  background-color: rgba(0, 0, 0, 0.5);
  margin: 5px;
  padding: 10px;
  list-style: none;
  border-radius: 5px;
  width: 150px;
  height: auto;
  color:seashell;
  box-shadow: -2px 2px 2px 1px black;
}

@media screen and (max-width: 1024px){

  #app {
    background-size: cover;
  }

  .forecast-box {
    flex-wrap: wrap;
  }

  .weather-box {
    width: auto;
    height: auto;
  }

  .forecast-wrap {
    margin: 20px;
    width: auto;
    height: auto;
  }
}
</style>
