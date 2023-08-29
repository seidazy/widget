<template>
  <div class="ChooseCities">
    <div class="ChooseCities_title">
      <span>Settings</span>
      <span class="mdi mdi-close" @click="closeChose"></span>
    </div>
    <draggable v-model="mountedCities" group="people" ghost-class="ghost" @start="dragging=true" @end="checkMove">
      <div v-for="city in mountedCities" :key="city.id" class="ChooseCities_city">
        <span class="mdi mdi-menu"></span>
        {{city.name}}
        <span class="mdi mdi-delete" @click="onDelete(city)"></span>
      </div>
    </draggable>
    <div class="ChooseCities_addLocation">
      Add location
      <input type="text" v-model="city" @keyup.enter="onAddCity">
      <span class="mdi mdi-arrow-left-bottom" @click="onAddCity"></span>
    </div>
  </div>
</template>
<script lang="ts">
import {Component, Vue, Prop, Watch} from 'vue-property-decorator';
import draggable from 'vuedraggable'

@Component({
  components: {draggable},
})
export default class ChooseCities extends Vue {
  city = ""
  dragging = false
  @Prop() cities: any
  mountedCities = []

  mounted() {
    this.mountedCities = this.cities
  }

  @Watch('cities')
  changeMountedCities() {
    this.mountedCities = this.cities
  }

  checkMove() {
    this.$emit('onCheckMove', this.mountedCities)
  }

  onDelete(city: any) {
    this.$emit('onDelete', city)
  }

  closeChose() {
    this.$emit('onCloseChose')
  }

  onAddCity() {
    this.$emit('onAddCity', this.city)
    this.city = ""
  }
}
</script>

<style lang="scss" scoped>
.ChooseCities {
  padding: 15px;
  background-color: white;

  &_title {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
  }

  &_city {
    cursor: move;
    background-color: rgba(0,0,0, 0.1);
    display: flex;
    justify-content: space-between;
    padding: 10px;
    margin-bottom: 10px;

    &:hover {
      box-shadow: 0 0 5px rgba(0,0,0,0.4);
    }
  }

  &_addLocation {
    text-align: left;
  }
}
</style>