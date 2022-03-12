<template>
  <div class="starship" @click.prevent="openStarship">
    <img class="image" :src="imageUrl">
    <div class="title">
      {{ starship.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'StarshipComponent',
  // eslint-disable-next-line vue/require-default-prop
  props: { url: String },
  data: () => ({
    starship: {},
    imageUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const starship = await this.$axios.get(this.url)
    this.starship = starship.data
    console.log(this.starship)
    let result
    try {
      const starshipId = this.starship.url.split('/')[5]
      result = require(`~/assets/img/starships/${starshipId}.jpg`)
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
    openStarship () {
      this.$router.push('/starship/' + this.starship.url.split('/')[5])
    }
  }
}
</script>

<style scoped>
.film{
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
