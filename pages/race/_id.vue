<template>
  <div>
    <div class="raceProfile">
      <CommonImage path="species" />
      <div class="infoBlock">
        <div class="name">
          {{ race.name }}
        </div>
        <div>Classification: {{ race.classification }}</div>
        <div>Average lifespan: {{ race.average_lifespan }}</div>
        <div>Average height: {{ race.average_height }}</div>
        <div>Eye colors: {{ race.eye_colors }}</div>
        <div>Hair colors: {{ race.hair_colors }}</div>
        <div>Language: {{ race.language }}</div>
        <div>Skin colors: {{ race.skin_colors }}</div>
        <div class="link" @click.prevent="openPlanet">
          Homeworld: {{ planet }}
        </div>
      </div>
    </div>
    <div class="additionalInformationWrapper">
      <div v-show="films" class="blockWrapper">
        <span class="blockTitle">Films</span>
        <div class="itemWrapper">
          <div v-for="film in race.films" :key="film">
            <Film :url="film" />
          </div>
        </div>
      </div>
      <div v-show="people" class="blockWrapper">
        <span class="blockTitle">People</span>
        <div class="itemWrapper">
          <div v-for="person in race.people" :key="person">
            <Resident :url="person" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CommonImage from '~/components/CommonImage'
import Film from '~/components/Film'
import Resident from '~/components/Resident'

export default {
  name: 'IdRace',
  components: {
    CommonImage,
    Resident,
    Film
  },
  data: () => ({
    race: {},
    planet: null,
    planetUrl: null,
    people: null,
    films: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const race = await this.$axios.get('https://swapi.dev/api/species/' + this.$route.params.id)
    const planet = await this.$axios.get(race.data.homeworld)
    this.planet = planet.data.name
    this.planetUrl = planet.data.url
    this.race = race.data
    this.people = this.race.people.length
    this.films = this.race.films.length
    this.$store.commit('loading', false)
  },
  methods: {
    openPlanet () {
      const planetId = this.planetUrl.split('/')[5]
      this.$router.push('/planet/' + planetId)
    }
  }
}
</script>

<style scoped>
.raceProfile{
  display: flex;
  justify-content: flex-start;
  margin-top: 100px;
  border: 1px solid red;
  border-radius: 10px;
  padding: 10px;
  width: 100%;
}
.link{
  cursor: pointer;
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
