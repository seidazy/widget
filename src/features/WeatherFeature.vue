<template>
  <div class="WeatherFeature">
    <span class="mdi mdi-cog WeatherFeature_settings" @click="onSettingsClick"></span>
    <ChooseCities
        class="WeatherFeature_action"
        :cities="cities"
        @onDelete="onDelete"
        @onAddCity="onAddCity"
        @onCheckMove="checkMove"
        @onCloseChose="closeChoose"
        v-if="showChoose"
    />
    <WeatherComponent v-for="cityWeather in sortedCities" :weatherInfo="cityWeather.weather" :key="cityWeather.name"/>
    <div class="WeatherFeature_empty" v-if="!sortedCities.length">
      add cities clicking to settings icon
    </div>
  </div>
</template>

<script lang="ts">
import {Component, Vue} from 'vue-property-decorator';
import draggable from 'vuedraggable'
import axios from 'axios';
import WeatherComponent from "@/components/WeatherComponent.vue";
import ChooseCities from "@/components/ChooseCities.vue";

@Component({
  components: { ChooseCities, WeatherComponent, draggable}
})
export default class WeatherFeature extends Vue{
  weatherInfo: any = {};
  ids = 0;
  citiesWeather: any = [];
  cities: any = JSON.parse(localStorage.getItem('cities') as string) || []
  showChoose = false

  onDelete(city: any) {
    let refactoredCities = this.cities.filter((cities: any) => cities.name.toUpperCase() !== city.name.toUpperCase())
    this.citiesWeather = this.citiesWeather.filter((cities: any) => cities.name.toUpperCase() !== city.name.toUpperCase())
    this.ids = JSON.parse(localStorage.getItem('cities') as string)?.length - 1;
    this.cities = refactoredCities
    localStorage.setItem('cities', JSON.stringify(refactoredCities))
  }

  get sortedCities() {
    return this.citiesWeather.sort((a: any, b: any) => a.id - b.id)
  }

  closeChoose() {
    this.showChoose = false
  }

  onSettingsClick() {
    this.showChoose = true
  }

  checkMove(changedCities: any) {
    this.cities = changedCities
    for(let i = 0; i < this.cities.length; i++) {
      this.cities[i].id = i;

      this.citiesWeather.map((city: any) => {
        if (city.name.toUpperCase() === this.cities[i].name.toUpperCase()) {
          return city.id = i
        }
      })
    }

    localStorage.setItem('cities', JSON.stringify(changedCities))
  }

  onAddCity(cityName: string) {
    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${cityName}&appid=0889950d4256cc7b43d43db8cafca8f9&units=metric`).then((res)=> {
      if (this.cities) {
        let isCityExists = this.cities.find((city : any) => city.name.toUpperCase() === cityName.toUpperCase())
        if (!isCityExists)  {
          this.cities.push({
            name: cityName,
            id: this.ids = (JSON.parse(localStorage.getItem('cities') as string)?.length) ?
                JSON.parse(localStorage.getItem('cities') as string)?.length : -1 + 1})
          localStorage.setItem('cities', JSON.stringify(this.cities))

          let weatherOfCity =  {
            id: this.ids,
            name: res.data.name,
            weather: res.data
          }
          this.citiesWeather.push(weatherOfCity);
        } else {
          alert("This city already exists")
        }
      } else {
        let city = [{name: cityName, id: 0}]
        this.cities = city
        localStorage.setItem('cities', JSON.stringify(city))
      }
    }).catch(() => {
      alert("There are no city")
    })
  }

  mounted() {
    for(let i = 0;i < this.cities.length; i++) {
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.cities[i].name}&appid=0889950d4256cc7b43d43db8cafca8f9&units=metric`).then((res)=> {
        this.weatherInfo = res.data
        let weatherOfCity =  {
          id: i,
          name: res.data.name,
          weather: res.data
        }

        this.citiesWeather.push(weatherOfCity);
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.WeatherFeature {
  width: 250px;
  position: relative;
  box-shadow: 0 0 5px rgba(0,0,0,0.4);

  &_cities {
    position: absolute;
    width: 100%;
    background-color: white;
    z-index: 5000;

  }

  &_settings {
    position: absolute;
    top: 10px;
    right: 10px;
    z-index: 99;
  }

  &_action {
    position: absolute;
    z-index: 100;
    box-shadow: 0 0 5px rgba(0,0,0,0.4);
  }

  &_empty {
    padding: 30px;
  }
}
</style>