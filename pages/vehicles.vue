<template>
  <div>
    <div class="paginatorWrapper">
      <div v-show="previousUrl !== null" class="button" @click.prevent="pushPreviousUrl(previousUrl)">
        Previous
      </div>
      <div v-show="nextUrl !== null" class="button" @click.prevent="pushNextUrl(nextUrl)">
        Next
      </div>
    </div>
    <ul class="vehiclesWrapper">
      <li v-for="vehicle in vehicles" :key="vehicle.name">
        <div href="#" class="vehicleName" @click.prevent="openVehicle(vehicle)">
          <div class="vehicleWrapper">
            <Vehicle vehicle-trigger :url="vehicle.url" />
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import Vehicle from '~/components/Vehicle'
export default {
  name: 'VehiclesPage',
  components: {
    Vehicle
  },
  data: () => ({
    vehicles: {},
    url: 'https://swapi.dev/api/vehicles?page=1',
    nextUrl: null,
    previousUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const vehicles = await this.$axios.get(this.url)
    this.vehicles = vehicles.data.results
    this.nextUrl = vehicles.data.next
    this.$store.commit('loading', false)
  },
  methods: {
    openVehicle (vehicle) {
      const vehicleId = vehicle.url.split('/')[5]
      this.$router.push('/vehicle/' + vehicleId)
    },
    async pushNextUrl (url) {
      this.$store.commit('loading', true)
      this.url = url
      const vehicles = await this.$axios.get(this.url)
      this.vehicles = vehicles.data.results
      this.nextUrl = vehicles.data.next
      this.previousUrl = vehicles.data.previous
      this.$store.commit('loading', false)
    },
    async pushPreviousUrl (url) {
      this.$store.commit('loading', true)
      this.url = url
      const vehicles = await this.$axios.get(this.url)
      this.vehicles = vehicles.data.results
      this.nextUrl = vehicles.data.next
      this.previousUrl = vehicles.data.previous
      this.$store.commit('loading', false)
    }
  }
}
</script>

<style>
li {
  list-style-type: none;
}

.vehiclesWrapper {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}

.vehicleWrapper {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  margin: 10px;
}

.vehicleName {
  color: red;
  text-decoration: none;
  cursor: pointer;
  font-size: 20px;
  font-weight: 600;
}
.button{
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  height: 30px;
  color: red;
  margin-right: 15px;
  border: 1px solid red;
  border-radius: 10px;
  padding: 10px;
}
.paginatorWrapper{
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin: 50px 0;
}
</style>
