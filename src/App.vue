<!--
    Name: Chase Cook
    Assignment: Final Project - Current Weather
    Class: Web Dev II Markley
-->

<template>

<div class="container" id="app">
  <Header />
  <div class="container" id="mainDiv">
    <form>
      <div class="container">
        <div class="mb-3">
          <input type="text" class="form-control" placeholder="Enter zip code..." v-model="zip">
        </div>
        <p id="pError">{{error}}</p>
        <div class="form-check">
          <input class="form-check-input" type="radio" value="F" v-model="unit" v-on:change="convertTemp">
          <label class="form-check-label" id="labels">Fahrenheit</label>
        </div>
        <div class="form-check">
          <input class="form-check-input" type="radio" value="C" v-model="unit" v-on:change="convertTemp">
          <label class="form-check-label" id="labels">Celsius</label>
          <button type="button" class="btn btn-outline-dark" v-on:click="validateZip" id="submitBtn"><b>View
              Weather</b></button>
        </div>
      </div>
    </form>
    <div v-if="error == null && temp != null">
      <Card v-bind:pIcon="icon" v-bind:pTemp="temp" v-bind:pUnit="unit" v-bind:pCity="city"
        v-bind:pConditions="conditions" />
    </div>
  </div>
</div>

</template>

<script>
import Header from './components/Header.vue'
import Card from './components/Card.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Header,
    Card
  },
  data()
  {
    return {
      // stored info
      temp: null,
      tempData: null,
      error: null,
      zip: '',
      unit: 'F',
      icon: '',
      city: '',
      conditions: '',
      regex: /^\d{5}$/
    }
  },
  methods: {
    validateZip() {
      // validates user input zip code with regex
      if (this.regex.test(this.zip) == true) {
        this.mounted()
        this.error = null
      }
      // checks if zip is empty and provides error message
      else if (this.zip == '') {
        this.error = 'Enter a zip code'
      }
      // if nothing else then display this error message
      else {
        this.error = 'Zip code must contain five numbers'
      }
    },
    // method converts temp from kelvin to fahrenheit or celsius live
    convertTemp() {
      if (this.tempData != null) {
        if (this.unit == 'F') {
          // formula for kelvin to fahrenheit then applied to this.temp
          this.temp = ((this.tempData - 273.15) * 9 / 5 + 32).toFixed(0)
        }
        else {
          // formula for kelvin to celsius then applied to this.temp
          this.temp = (this.tempData - 273.15).toFixed(0)
        }
      }
    },
    mounted() {
      // erases previous data
      this.temp = null
      // axios api call in order to get the weather data, also plugs the user input zip code into the url
      axios.get('https://api.openweathermap.org/data/2.5/weather?zip=' + this.zip + '&appid=23c33d5e651bf3ed04d85783f4e1ac18')
      .then((response) => {
        // weather info into variables
        this.icon = response.data.weather[0].icon
        this.tempData = response.data.main.temp
        this.convertTemp()
        this.city = response.data.name
        this.conditions = response.data.weather[0].main
      })
      // message for api data error
      .catch(() => {
        this.error = "Invalid zip"
      })
    }
  }
}
</script>

<style>
html > body
{
  background: #2A2A2E;
}

#mainDiv
{
  background: #38383D;
  margin-top: 20px;
  margin-bottom: 50px;
  border-radius: 10px;
  padding-bottom: 10px;
  padding-top: 50px;
  width: 600px;
}

#app
{
  margin-top: 50px;
  margin-bottom: 50px;
  border-radius: 10px;
  padding-bottom: 10px;
}

#submitBtn
{
  background: rgb(172, 255, 172);
  color: black;
  float: right;
}

form
{
  padding-bottom: 20px;
  padding-bottom: 50px;
  width: 500px;
  margin: auto;
}

#pError
{
  color: red;
}

#labels
{
  color: honeydew;
}
</style>

