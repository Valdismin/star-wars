<template>
  <div class="vehicle" @click.prevent="openVehicle">
    <img :class="{vehicleImage: vehicleTrigger}" class="image" :src="imageUrl">
    <div :class="{vehicleTitle: vehicleTrigger}" class="title">
      {{ vehicle.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'VehicleComponent',
  props: { url: String, vehicleTrigger: Boolean },
  data: () => ({
    vehicle: {},
    imageUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const vehicle = await this.$axios.get(this.url)
    this.$store.commit('loading', false)
    this.vehicle = vehicle.data
    let result
    try {
      const vehicleId = this.vehicle.url.split('/')[5]
      result = require(`~/assets/img/vehicles/${vehicleId}.jpg`)
    } catch (e) {
      if (e.code !== 'MODULE_NOT_FOUND') {
        throw e
      }
      result = require('~/assets/img/placeholder.jpg')
    }
    this.imageUrl = result
  },
  methods: {
    openVehicle () {
      this.$router.push('/vehicle/' + this.vehicle.url.split('/')[5])
    }
  }
}
</script>

<style scoped>
.vehicle{
  padding: 10px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}
.image{
  width: 100px;
  height: 80px;
}
.title{
  color: red;
  font-size: 14px;
  font-weight: 500;
}
.vehicleTitle{
  font-size: 20px;
  font-weight: 600;
}
.vehicleImage{
  width: 200px;
  height: 200px;
}
</style>
