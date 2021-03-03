<template>
  <div id="app">
    <div id="wrapperDiv" class="wrapper">
      <img :src=weatherURL id="weatherImg" />
      <section id="topbar" class="topBar scroll-right">
        <p id="topBarText" class="topBarText">Showing weather in: {{ city }}</p>
      </section>
      <br />

      <div id="textArea">
        <p id="weatherP" class="centerText">
          {{ tempValue }}
        </p>
        <div id="buttons" class="buttonDiv">
          <p id="temp" class="centerText">Temperature: {{ tempValue }}{{celFarShort}}</p>
          <button id="change" @click="changeTemp" class="centerText">
            <p>To {{ temp }}</p>
          </button>
          <p id="test">Made with OpenweatherAPI</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import $ from 'jquery';
import cloudy from './assets/cloudy.jpg';
import mist from './assets/mist.jpg';
import rainy from './assets/rain.jpg';
import sunny from './assets/sunny.jpg';
import snowy from './assets/snow.jpg';
export default {
  name: 'weather-app',
  data: function () {
    return {
      value: 0,
      temp: 'Farenheit',
      tempValue: '0',
      url: '',
      city: '',
      weather: '',
      weatherURL: '',
      celFarShort: 'C'
    };
  },
  props: {},
  created() {
    //document.getElementById("weatherImg").src = "assets/sunny.jpg";
  },
  mounted() {
    var url = this.getJson();
    this.makeDataModel(url);
  },
  methods: {
    getJson: function () {
      var self = this;
      $.getJSON(
        //replace ipinfo url
        function (a) {
          var location = a.loc;
          var locationArray = location.split(',');
          var aa = locationArray[0];
          var bb = locationArray[1];
          self.city = a.city + ', ' + a.country;
          //replace for openweather api url
          var url =
            //https://cors-anywhere.herokuapp.com/
            'http://api.openweathermap.org/data/2.5/weather?lat=' +
            aa +
            '&' +
            'lon=' +
            bb +
            '&appid=     replace   ';
          self.url = url;
          $.getJSON(url, function (result) {
            var dataModel = {};
            var array = result; // local var
            var weatherObj = array['weather']; //weather obj
            self.changeBG(weatherObj[0].main); //displays correct bg
            document.getElementById('weatherP').innerHTML =
              'Weather: ' +
              '<br>' +
              weatherObj[0].main +
              ',' +
              weatherObj[0].description;
            //temperatures and conversion.
            var int = parseFloat(array['main'].temp);
            var cel = int - 273.5;
            var faren;
            cel = Math.ceil(cel);
            faren = cel + 32;
            var farStr = 'Temperature: ' + faren + 'F';
            var celStr = 'Temperature: ' + cel + 'C';
            self.tempValue = cel;
            dataModel.farStr = farStr;
            dataModel.celStr = celStr;
          });
        }
      );
    },
    makeDataModel: function (url) {
      $.getJSON(url, function (result) {
        var dataModel = {};
        var array = result; // local var
        var weatherObj = array['weather']; //weather obj
        this.changeBG(weatherObj[0].main); //displays correct bg
        document.getElementById('weatherP').innerHTML =
          'Weather: ' +
          '<br>' +
          weatherObj[0].main +
          ',' +
          weatherObj[0].description;
        //temperatures and conversion.
        var int = parseFloat(array['main'].temp);
        var cel = int - 273.5;
        var faren;
        cel = Math.ceil(cel);
        faren = cel + 32;
        var farStr = 'Temperature: ' + faren + 'F';
        var celStr = 'Temperature: ' + cel + 'C';
        dataModel.farStr = farStr;
        dataModel.celStr = celStr;
      });
    },
    changeTemp: function () {
      if (this.temp === 'Farenheit') {
        this.tempValue += 32;
        this.celFarShort = 'F'
        this.temp = "Celcius";
        
      } else if (this.temp === 'Celcius') {
        this.tempValue -= 32;
        this.celFarShort = 'C';
        this.temp = "Farenheit";
      }
    },
    changeBG: function (str) {
      /*
      var clear = "url('../img/sunny.jpg')";
      var rain = "url('../rain.jpg')";
      var cloudy = "url('../cloudy.jpg')";
      var snow = "url('../snow.jpg')";
      */
      //document.createElement('img');
      console.log(str);
      if (str === 'Clear') {
        this.weatherURL = sunny;
      } else if (str === 'Clouds') {
        this.weatherURL = cloudy;
      } else if (str === 'Snow') {
        this.changeTextColor();
        this.weatherURL = snowy;
      } else if (str === 'Rain' || str === 'Drizzle') {
        this.weatherURL = rainy;
        this.changeTextColor();
      } else if (str === 'Mist') {
        this.weatherURL = mist;
      }
      document.body.style.backgroundSize = 'cover';
    },
    changeTextColor: function () {
      document.getElementById('weatherP').style.color = 'white';
      document.getElementById('temp').style.color = 'white';
    },
  },
};
</script>

<style>
.wrapper {
  margin-top: -50px;
  margin: auto;
  width: 900px;
  padding: 3px;
  border-radius: 15px;
  height: 550px;
}
img {
  opacity: 0.7;
  position: absolute;
  z-index: -1;
  margin: auto;
  width: 900px;
  border-radius: 15px;
  height: 550px;
}
.topBar {
  margin-top: 5px;
  width: 50%;
  z-index: 10000;
  position: relative;
  top: 0;
  margin-left: 25%;
  border-radius: 25px;
  background-color: lightblue;
  text-align: center;
}
button{
  border-radius: 25px;
  width:130px;
  height:50px;
  background-color:white;
  border-color:orange;
  border: 2px solid orange;
}
button:hover {
  background-color: #65a9d7;;
}
.topBarText {
  font-family: Avantgarde, sans-serif;
  font-size: 120%;
  padding: 0px;
}
h2 {
  text-align: center;
  position: relative;
  font-family: 'Arial Black', Gadget, sans-serif;
  font-size: 300%;
}
.centerText {
  text-align: center;
  position: relative;
  border-radius: 15px;
  font-family: 'Arial Black', Gadget, sans-serif;
  font-size: 250%;
  z-index: 3;
}
.buttonDiv {
  text-align: center;
  margin-left: 100px;
  margin-right: 100px;
  padding: -5px;
}
#change {
  text-align:center;
  color: #65a9d7;
  font-size:100%;
  background: -webkit-linear-gradient(top, #c7cacc, #2c2d2d);
  background: -moz-linear-gradient(top, #3e779d, #65a9d7);
  background: -ms-linear-gradient(top, #3e779d, #65a9d7);
  background: -o-linear-gradient(top, #3e779d, #65a9d7);
}
#change:hover{
  color:white;
}
#change:hover {
  background: -webkit-linear-gradient(top, #949da3, #000000);
  background: -moz-linear-gradient(top, #949da3, #000000);
  background: -ms-linear-gradient(top, #949da3, #000000);
  background: -o-linear-gradient(top, #949da3, #000000);
  border-top-color: #28597a;
}
#change:active {
  border-top-color: #28597a;
  background: -webkit-linear-gradient(top, #949da3, #000000);
  background: -moz-linear-gradient(top, #949da3, #000000);
  background: -ms-linear-gradient(top, #949da3, #000000);
  background: -o-linear-gradient(top, #949da3, #000000);
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}

.scroll-right {
  height: 50px;
  overflow: hidden;
}
.scroll-right #topBarText {
  width: 100%;
  height: 100%;
  text-align: center;
  /* Starting position */
  -moz-transform: translateX(-70%);
  -webkit-transform: translateX(-70%);
  transform: translateX(-70%);
  /* Apply animation to this element */
  -moz-animation: scroll-right 20s linear infinite;
  -webkit-animation: scroll-right 25s linear infinite;
  animation: scroll-right 15s linear infinite;
}
/* Move it (define the animation) */
@-moz-keyframes scroll-right {
  0% {
    -moz-transform: translateX(-50%);
  }
  100% {
    -moz-transform: translateX(90%);
  }
}
@-webkit-keyframes scroll-right {
  0% {
    -webkit-transform: translateX(-90%);
  }
  100% {
    -webkit-transform: translateX(90%);
  }
}
@keyframes scroll-right {
  0% {
    -moz-transform: translateX(-90%); /* Browser bug fix */
    -webkit-transform: translateX(-90%); /* Browser bug fix */
    transform: translateX(-90%);
  }
  100% {
    -moz-transform: translateX(90%); /* Browser bug fix */
    -webkit-transform: translateX(90%); /* Browser bug fix */
    transform: translateX(90%);
  }
}
</style>
