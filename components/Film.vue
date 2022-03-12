<template>
  <div class="film" @click.prevent="openFilm">
    <img class="image" :src="imageUrl">
    <div class="title">
      {{ film.title }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilmComponent',
  // eslint-disable-next-line vue/require-default-prop
  props: { url: String },
  data: () => ({
    film: {},
    imageUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const film = await this.$axios.get(this.url)
    this.film = film.data
    let result
    try {
      const filmId = this.film.url.split('/')[5]
      result = require(`~/assets/img/films/${filmId}.jpg`)
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
    openFilm () {
      this.$router.push('/film/' + this.film.episode_id)
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
  height: 130px;
}
.title{
  color: red;
  font-size: 14px;
  font-weight: 500;
}
</style>
