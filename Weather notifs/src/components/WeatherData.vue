<template>
  <div class = "flexContainer" >
    <h1 class = "errorText" v-if=error style = "color:red">Error receiving Weather Data!</h1>
    <div class="iconContainer">
      <Thermometer/>
      <p> Ambient<br>Temperature</p>
    </div>
    <p class = "infoText"> {{temp}} </p>
    <div class="iconContainer">
      <Windsock/>
      <p> Wind<br>Direction </p>
    </div>
    <p class = "infoText"> {{windDirection}} </p>
    <div class="iconContainer">
      <WeatherWindy/>
      <p> Wind<br>Speed </p>
    </div>
    <p class = "infoText"> {{windSpeed}} </p>
    <div class="iconContainer">
      <WaterPercent/>
      <p> Relative<br>Humidity </p>
    </div>
    <p class = "infoText"> {{humidity}} </p>
    <div class="iconContainer">
      <WeatherPouring/>
      <p> Rain </p>
    </div>
    <p class = "infoText"> {{rain}}</p>
  </div>
</template>

<script>
import Thermometer from 'vue-material-design-icons/Thermometer.vue';
import Windsock from 'vue-material-design-icons/Windsock.vue';
import WeatherWindy from 'vue-material-design-icons/WeatherWindy.vue';
import WaterPercent from 'vue-material-design-icons/WaterPercent.vue';
import WeatherPouring from 'vue-material-design-icons/WeatherPouring.vue';
import 'vue-material-design-icons/styles.css';
import axios from 'axios';

// const api = 'mNf8SgViuZwsI0zKUrzmZDzSSTCF3gpQ';

export default {
  name: 'WeatherData',
  components: {
    Thermometer,
    Windsock,
    WeatherWindy,
    WaterPercent,
    WeatherPouring,
  },
  data() {
    return {
      temp: null,
      windDirection: null,
      windSpeed: null,
      humidity: null,
      rain: null,
      error: false,
    };
  },
  props: {
    stationID: {
      type: String,
      default: '93000',
    },
    APIKey: {
      type: String,
      default: '',
    },
    APISecret: {
      type: String,
      default: '',
    },
  },
  methods: {
    async fetchNews() {
      try {
        this.error = null;
        this.loading = true;
        // Didn't include the key queries since their included by default on the backend
        const url = `http://api.conxedge.com/weather/${this.stationID}`;
        const params = {
          api_key: this.APIKey,
          api_secret: this.APISecret,
        };
        const response = await axios.get(url, { params });
        const { temperature, humidity, windspeed } = response.data;
        this.temp = `${temperature}°C`;
        this.humidity = `${humidity}%`;
        this.windSpeed = `${windspeed} km/h`;
        this.windDirection = 'NW';
        this.rain = '75mm';
      } catch (err) {
        this.error = true;
        if (err.response) {
          // client received an error response (5xx, 4xx)
          console.log('Server Error:', err);
        } else if (err.request) {
          // client never received a response, or request never left
          console.log('Network Error:', err);
        } else {
          console.log('Client Error:', err);
        }
      }
    },
  },
  mounted() {
    // initial call
    this.fetchNews();
    // fetch every 300 seconds(5 minutes)
    window.setInterval(() => {
      this.fetchNews();
      console.log('sent request');
    }, 300000);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .flexContainer{
    display:flex;
    flex-direction:row;
    justify-content:center;
    gap: 2%;
    padding: 0px 10px
  }
  .iconContainer{
    display:flex;
    flex-direction:column;
    font-size: 2.0vw;
    color: #326878;
  }

  .iconContainer p{
    font-size: 0.7vw;
    color: #414141;
  }

  .infoText{
    display:flex;
    flex-direction:row;
    justify-content:center;
    font-size: 1vw;
    align-items: center;
    justify-content: center;
    margin-left: -0.5vw;
    margin-right: 0.5vw;
    white-space: nowrap;
  }

  .errorText{
    font-size: 1vw;
    display: flex;
    align-items: center;
    justify-content:center;
  }

  @media screen and (max-width: 800px) {
    .iconContainer p{
      font-size: 0vw;
    }

    .iconContainer{
      font-size: 3.5vw;
      margin-bottom: 3px;
    }

    .errorText{
      font-size: 1.5vw;
      justify-content:center;
    }

    .infoText{
      font-size: 1.5vw
    }

  }

  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
</style>
