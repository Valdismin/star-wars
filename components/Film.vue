<template>
  <div class="film" @click.prevent="openFilm">
    <img :class="{filmImage: filmsTrigger}" class="image" :src="imageUrl">
    <div :class="{filmTitle: filmsTrigger}" class="title">
      {{ film.title }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilmComponent',
  // eslint-disable-next-line vue/require-default-prop
  props: { url: String, filmsTrigger: Boolean },
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
      const filmId = this.film.url.split('/')[5]
      this.$router.push('/film/' + filmId)
    }
  }
}
</script>

<style scoped>
.film{
  padding: 10px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.image{
  width: 100px;
  height: 130px;
}
.filmImage{
  width: 200px;
  height: 260px;
}
.title{
  color: red;
  font-size: 14px;
  font-weight: 500;
}
.filmTitle{
  font-size: 20px;
  font-weight: 600;
}
</style>
