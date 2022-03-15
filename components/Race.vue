<template>
  <div class="race" @click.prevent="openRace">
    <img class="raceImage" :src="imageUrl">
    <div class="raceTitle">
      {{ race.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'PlanetComponent',
  // eslint-disable-next-line vue/require-default-prop
  props: { url: String },
  data: () => ({
    race: {},
    imageUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const race = await this.$axios.get(this.url)
    this.race = race.data
    let result
    try {
      const raceId = this.race.url.split('/')[5]
      result = require(`~/assets/img/species/${raceId}.jpg`)
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
    openRace () {
      this.$router.push('/planet/' + this.planet.url.split('/')[5])
    }
  }
}
</script>

<style scoped>
.race{
  padding: 10px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.raceTitle{
  color: red;
  font-size: 20px;
  font-weight: 600;
}
.raceImage{
  width: 200px;
  height: 200px;
}
</style>
