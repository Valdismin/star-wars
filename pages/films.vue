<template>
  <div>
    <ul class="filmsWrapper">
      <li v-for="film in films" :key="film.title">
        <div href="#" class="filmName" @click.prevent="openFilm(film)">
          <div class="filmWrapper">
            <Film films-trigger :url="film.url" />
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import Film from '~/components/Film'

export default {
  name: 'FilmsPage',
  components: {
    Film
  },
  data: () => ({
    films: {}
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const films = await this.$axios.get('https://swapi.dev/api/films/')
    this.films = films.data.results
    this.$store.commit('loading', false)
  },
  methods: {
    openFilm (film) {
      const filmId = film.url.split('/')[5]
      this.$router.push('/film/' + filmId)
    }
  }
}
</script>

<style>
li {
  list-style-type: none;
}

.filmsWrapper {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}

.filmWrapper {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  margin: 10px;
}

.filmName {
  color: red;
  text-decoration: none;
  cursor: pointer;
  font-size: 20px;
  font-weight: 600;
}
</style>
