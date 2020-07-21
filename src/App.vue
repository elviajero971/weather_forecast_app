<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 18 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temps">
            <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
            <div class="temp_min_max">{{ Math.round(weather.main.temp_min) }} / {{ Math.round(weather.main.temp_max) }} °c</div>
          </div>
          <div class="weather-description"><i class="fas fa-cloud-sun fa-3x"></i></div>
          <div class="weather-description">{{ weather.weather[0].main }}</div>
          <div class="weather">
            <div class="pressure-humidity">
              <div><i class="fas fa-tint"></i> {{ weather.main.humidity }}%</div>  
              <div>{{ weather.main.pressure }}hPa</div>
            </div>
            <div class="wind">
              <div><i class="fas fa-wind"></i> {{ Math.round(weather.wind.speed*3,6) }}km/h</div> 
              <div><i class="far fa-compass"></i> {{ Math.round(weather.wind.deg) }}°</div>
            </div>
          </div>
        </div>
        <div class="weather-forecast">
          <div class="weather-d">
            <div class="weather"><i class="fas fa-cloud-sun"></i></div>
            18 / 23°C
            <div class="date">{{ dateForcastOneBuilder() }}</div>
          </div>
          <div class="weather-d">
            <div class="weather"><i class="fas fa-cloud-sun"></i></div>
            18 / 23°C
            <div class="date">{{ dateForcastTwoBuilder() }}</div>
          </div>
          <div class="weather-d">
            <div class="weather"><i class="fas fa-cloud-sun"></i></div>
            18 / 23°C
            <div class="date">{{ dateForcastThreeBuilder() }}</div>
          </div>
          <div class="weather-d">
            <div class="weather"><i class="fas fa-cloud-sun"></i></div>
            18 / 23°C
            <div class="date">{{ dateForcastFourBuilder() }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: '782590f8e978196668e1009710e1f9a1',
      url_base: "http://api.openweathermap.org/data/2.5/",
      query:'',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather =results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["Janvier", "Février", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"];
      let days = ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },

    dateForcastOneBuilder () {
      let d = new Date()
      let days = ["Dim.", "Lun.", "Mar.", "Mer.", "Jeu.", "Ven.", "Sam."];
      let day = days[d.getDay() + 1];
      return `${day}`;
    },

    dateForcastTwoBuilder () {
      let d = new Date()
      let days = ["Dim.", "Lun.", "Mar.", "Mer.", "Jeu.", "Ven.", "Sam."];
      let day = days[d.getDay() +2];
      return `${day}`;
    },

    dateForcastThreeBuilder () {
      let d = new Date()
      let days = ["Dim.", "Lun.", "Mar.", "Mer.", "Jeu.", "Ven.", "Sam."];
      let day = days[d.getDay() +3];
      return `${day}`;
    },

    dateForcastFourBuilder () {
      let d = new Date()
      let days = ["Dim.", "Lun.", "Mar.", "Mer.", "Jeu.", "Ven.", "Sam."];
      let day = days[d.getDay()+3];
      return `${day}`;
    },

    windDirection () {
    }
  }
}

</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  width: 600px;
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border:none;
  outline:none;
  background:none;

  box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight:900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .temp_min_max {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  font-style: normal;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather-description {
  color: #FFF;
  font-size: 40px;
  font-weight: 600;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  display: flex;
  align-items:center;
  justify-content: center;

  padding: 10px;
}

.weather-box .pressure-humidity {
  color: #FFF;
  font-size: 30px;
  font-weight: 200;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  padding: 10px;
  margin: 10px;



}

.weather-box .wind {
  color: #FFF;
  font-size: 30px;
  font-weight: 200;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 10px;
  padding: 10px;
  
}

.weather-forecast {
  display: inline-block;
  align-items: flex-end;
}

.weather-forecast .weather-d {
  text-align: center;
  display: inline-block;
  justify-items: center;
  padding: 4px;
  margin-top: 100px;
  margin-left: 5px;
  margin-right: 5px;
  color: #FFF;
  font-size: 25px;
  font-weight:$00;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
}
</style>
