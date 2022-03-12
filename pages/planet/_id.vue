<template>
  <img class="image" :src="imageUrl">
</template>

<script>
export default {
  name: 'IdPlanet',
  data: () => ({
    name: null,
    imageUrl: null
  }),
  async created () {
    const planet = await this.$axios.get('https://swapi.dev/api/planets/' + this.$route.params.id)
    this.name = planet.data.name
    let result
    try {
      result = require(`~/assets/img/planets/${this.$route.params.id}.jpg`)
    } catch (e) {
      if (e.code !== 'MODULE_NOT_FOUND') {
        throw e
      }
      result = require('~/assets/img/placeholder.jpg')
    }
    this.imageUrl = result
  }
}
</script>

<style scoped>
.image {
  height: 100px;
  width: 100px;
}
</style>
