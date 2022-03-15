<template>
  <div>
    <div class="filmProfile">
      <CommonImage path="films" />
      <div class="infoBlock">
        <div class="name">
          {{ film.title }}
        </div>
        <div>Director: {{ film.director }}</div>
        <div>Release date: {{ film.release_date }}</div>
        <div>Producer: {{ film.producer }}</div>
        <div class="crawl">
          {{ film.opening_crawl }}
        </div>
      </div>
    </div>
    <div class="additionalInformationWrapper">
      <div v-show="starships" class="blockWrapper">
        <span class="blockTitle">Starships</span>
        <div class="itemWrapper">
          <div v-for="starship in film.starships" :key="starship">
            <Starship :url="starship" />
          </div>
        </div>
      </div>
      <div v-show="vehicles" class="blockWrapper">
        <span>Related vehicles</span>
        <div class="itemWrapper">
          <div v-for="vehicle in film.vehicles" :key="vehicle.url">
            <Vehicle :url="vehicle" />
          </div>
        </div>
      </div>
      <div class="blockWrapper">
        <span class="blockTitle">Characters</span>
        <div class="itemWrapper">
          <div v-for="character in film.characters" :key="character">
            <Resident :url="character" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CommonImage from '~/components/CommonImage'

export default {
  name: 'IdFilm',
  components: {
    CommonImage
  },
  data: () => ({
    film: {},
    vehicles: null,
    starships: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const film = await this.$axios.get('https://swapi.dev/api/films/' + this.$route.params.id)
    this.film = film.data
    this.vehicles = this.film.vehicles.length
    this.starships = this.film.starships.length
    this.$store.commit('loading', false)
  },
  methods: {
    openPlanet () {
      const planetId = this.planetUrl.split('/')[5]
      this.$router.push('/planet/' + planetId)
    },
    openSpecies () {
      const speciesId = this.hero.species[0].split('/')[5]
      this.$router.push('/species/' + speciesId)
    }
  }
}
</script>

<style scoped>
.crawl{
  margin-top: 30px;
}
.filmProfile{
  display: flex;
  justify-content: flex-start;
  margin-top: 100px;
  border: 1px solid red;
  border-radius: 10px;
  padding: 10px;
  width: 100%;
}
.name{
  color: red;
  text-decoration: none;
  cursor: pointer;
  font-weight: 700;
  font-size: 25px;
}
.infoBlock {
  color: red;
  padding-left: 20px;
  height: 300px;
}
.additionalInformationWrapper {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  padding-bottom: 100px;
}
.infoBlock {
  color: red;
  padding-left: 20px;
  height: 300px;
}

.itemWrapper {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.blockWrapper {
  height: 100%;
  width: 100%;
  color: red;
  font-size: 17px;
  font-weight: 600;
  margin-top: 50px;
  padding: 20px;
  border-radius: 10px;
  border: 1px solid red;
  margin-right: 20px;
}
.blockTitle{
  padding-bottom: 10px;
}
</style>
