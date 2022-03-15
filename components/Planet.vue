<template>
  <div class="planet" @click.prevent="openPlanet">
    <img class="planetImage" :src="imageUrl">
    <div class="planetTitle">
      {{ planet.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'PlanetComponent',
  // eslint-disable-next-line vue/require-default-prop
  props: { url: String },
  data: () => ({
    planet: {},
    imageUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const planet = await this.$axios.get(this.url)
    this.planet = planet.data
    let result
    try {
      const planetId = this.planet.url.split('/')[5]
      result = require(`~/assets/img/planets/${planetId}.jpg`)
    } catch (e) {
      if (e.code !== 'MODULE_NOT_FOUND') {
        throw e
      }
      result = require('~/assets/img/placeholder.jpg')
    }
    this.imageUrl = result
    this.$store.commit('loading', false)
  },
  methods: {
    openPlanet () {
      this.$router.push('/planet/' + this.planet.url.split('/')[5])
    }
  }
}
</script>

<style scoped>
.planet{
  padding: 10px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.planetTitle{
  color: red;
  font-size: 20px;
  font-weight: 600;
}
.planetImage{
  width: 200px;
  height: 200px;
}
</style>
