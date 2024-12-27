<script>
import axios from 'axios';

  export default {
    data() {
      return {
        city:'',
        error: '',
        info: null
      }
    },
    computed:{
      findWeather(){
        return `Узнать погоду в ${this.checkingСity()}`
      },
      temp(){
        return 'Температура: ' + this.info.main.temp
      },
      tempMax(){
        return 'Максимальная температура: ' + this.info.main.temp_max
      },
      tempMin(){
        return 'Минимальная температура: ' + this.info.main.temp_min
      }
    },
    methods: {
      checkingСity(){
        if (this.city == ''){
          return 'вашем городе'
        }
          return 'городе ' + this.city
      },
      async getWeather() {
        try {
          if (this.city.trim().length < 2) {
            this.error = 'Нужно название более 1 символа'
          return false
          }
            this.error = ''
            const { data } = await axios.get(`https://api.openweathermap.org/data/2.5/weather`, { params: {
              q: this.city, 
              units: 'metric',
              appId: 'cfaddfb075fa5baad230f2f4f628d3e3',
            }})
            this.info = data
        } catch (e) {
          console.error('GET WEATHER::', e)
          this.error = e
        }
      }
        
    },
  }
</script>

<template>
  <div class="wrapper">
    <h1>{{ findWeather }}</h1>
    <input v-model="city" type="text" placeholder="Название города">
    <button @click="getWeather()" v-show="city != ''">Найти</button>
    <p class="error">{{ error }}</p>
    <div v-if="info != null">
      <p>{{ temp }}</p>
      <p>{{ tempMax }}</p>
      <p>{{ tempMin }}</p>
      <a :href="`https://openweathermap.org/city/${info.id}`">Узнать подробнее</a>
    </div>
  </div>
</template>

<style scoped>
.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: rgb(21, 0, 116);
  padding: 20px;
  text-align: center;
  color: white;
}
.wrapper h1{
  margin-top: 50px;
}
.wrapper input{
  border: 0;
  margin-top: 30px;
  padding: 5px 8px;
  font-size: 14px;
  outline: none;

}
.wrapper button{
  padding: 5px 20px;
  margin-left: 10px;
  cursor: pointer;
  border-radius: 5px;
}
.error{
  color: red;
}
.wrapper p{
  font-size:20px;
  padding-top: 10px;
}
.wrapper a{
  color: rgb(255, 217, 0);
}
</style>
