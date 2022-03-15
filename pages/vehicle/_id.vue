<template>
  <div>
    <div class="vehicleProfile">
      <CommonImage path="vehicles" />
      <div class="infoBlock">
        <div class="name">
          {{ vehicle.name }}
        </div>
        <div>Vehicle class: {{ vehicle.vehicle_class }}</div>
        <div>Passengers: {{ vehicle.passengers }}</div>
        <div>Cargo capacity: {{ vehicle.cargo_capacity }}</div>
        <div>Passengers: {{ vehicle.passengers }}</div>
        <div>Max speed: {{ vehicle.max_atmosphering_speed }}</div>
        <div>Length: {{ vehicle.length }}</div>
      </div>
    </div>
    <div class="additionalInformationWrapper">
      <div v-show="films" class="blockWrapper">
        <span class="blockTitle">Films</span>
        <div class="itemWrapper">
          <div v-for="film in vehicle.films" :key="film">
            <Film :url="film" />
          </div>
        </div>
      </div>
      <div v-show="pilots" class="blockWrapper">
        <span class="blockTitle">Pilots</span>
        <div class="itemWrapper">
          <div v-for="pilot in vehicle.pilots" :key="pilot">
            <Resident :url="pilot" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CommonImage from '~/components/CommonImage'
import Film from '~/components/Film'
import Resident from '~/components/Resident'

export default {
  name: 'IdVehicle',
  components: {
    CommonImage,
    Film,
    Resident
  },
  data: () => ({
    vehicle: {},
    films: null,
    pilots: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const vehicle = await this.$axios.get('https://swapi.dev/api/vehicles/' + this.$route.params.id)
    this.vehicle = vehicle.data
    this.pilots = this.vehicle.pilots.length
    this.films = this.vehicle.films.length
    this.$store.commit('loading', false)
  }
}
</script>

<style scoped>
.vehicleProfile{
  display: flex;
  justify-content: flex-start;
  margin-top: 100px;
  border: 1px solid red;
  border-radius: 10px;
  padding: 10px;
  width: 100%;
}
.name{
  color: red;
  text-decoration: none;
  cursor: pointer;
  font-weight: 700;
  font-size: 25px;
}
.infoBlock {
  color: red;
  padding-left: 20px;
  height: 300px;
}
.additionalInformationWrapper {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  padding-bottom: 100px;
}
.infoBlock {
  color: red;
  padding-left: 20px;
  height: 300px;
}

.itemWrapper {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.blockWrapper {
  height: 100%;
  width: 100%;
  color: red;
  font-size: 17px;
  font-weight: 600;
  margin-top: 50px;
  padding: 20px;
  border-radius: 10px;
  border: 1px solid red;
  margin-right: 20px;
}
.blockTitle{
  padding-bottom: 10px;
}
</style>
