<template>
  <div class="WeatherComponent">
    <div class="WeatherComponent_city">
      {{ weatherInfo.name }}
    </div>
    <div class="WeatherComponent_degree">
      <img :src="`https://openweathermap.org/img/wn/${weatherIcon}@2x.png`" alt="">
      <span>{{ weatherInfo.main.temp }}° C</span>
    </div>
    <div class="WeatherComponent_feels">
      {{ weatherInfo.main.feels_like }}° C,
      {{ weatherInfo?.weather[0].description }}
    </div>
    <div class="WeatherComponent_info">
      <div class="WeatherComponent_wind">
        <span class="mdi mdi-navigation-variant"></span>
        <span>{{ weatherInfo.wind.speed }}m/s</span>
      </div>
      <div class="WeatherComponent_pressure">
        <span class="mdi mdi-car-brake-low-pressure"></span>
        {{ weatherInfo.main.pressure }}hPa
      </div>
      <div class="WeatherComponent_humidity">
        Humidity: {{ weatherInfo.main.humidity }}%
      </div>
      <div class="WeatherComponent_dew">
        DewPoint: 0° C
      </div>
      <div class="WeatherComponent_visibility">
        Visibility: {{ convertVisibility }}km
      </div>

    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

@Component
export default class WeatherComponent extends Vue {
  @Prop() weatherInfo?: any;

  get weatherIcon() {
    if (this.weatherInfo?.weather) {
      return this.weatherInfo?.weather[0].icon
    } else {
      return {}
    }
  }

  get convertVisibility() {
    return (this.weatherInfo?.visibility / 1000).toFixed(1);
  }
}
</script>

<style lang="scss" scoped>
.WeatherComponent {
  width: 250px;
  padding: 10px 15px;
  border-radius: 5px;
  box-shadow: 0 0 5px rgba(0,0,0, 0.4);
  position: relative;

  &_city {
    text-align: left;
    margin-bottom: 10px;
  }

  &_degree {
    display: flex;
    justify-content: space-around;
    align-items: center;

    img {
      width: 50%;
    }

    span {
      font-size: 28px;
      line-height: 28px;
      height: 20px;
    }
  }

  &_feels {
    font-size: 12px;
    padding-bottom: 10px;
  }

  &_info {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    font-size: 13px;
    font-weight: bold;
    column-gap: 5px;
    row-gap: 10px;

    div {
      width: calc(50% - 5px);
      text-align: left;
    }
  }
}
</style>