<template>
  <div class="vehicle" @click.prevent="openVehicle">
    <img class="image" :src="imageUrl">
    <div class="title">
      {{ vehicle.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'VehicleComponent',
  // eslint-disable-next-line vue/require-default-prop
  props: { url: String },
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
  width: 120px;
  padding: 10px;
  cursor: pointer;
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
</style>
