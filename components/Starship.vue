<template>
  <div class="starship" @click.prevent="openStarship">
    <img :class="{starshipImage: starshipTrigger}" class="image" :src="imageUrl">
    <div :class="{starshipTitle: starshipTrigger}" class="title">
      {{ starship.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'StarshipComponent',
  // eslint-disable-next-line vue/require-default-prop
  props: { url: String, starshipTrigger: Boolean },
  data: () => ({
    starship: {},
    imageUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const starship = await this.$axios.get(this.url)
    this.starship = starship.data
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
.starship{
  padding: 10px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
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
.starshipTitle{
  font-size: 20px;
  font-weight: 600;
}
.starshipImage{
  width: 200px;
  height: 200px;
}
</style>
