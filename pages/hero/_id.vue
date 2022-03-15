<template>
  <div class="heroPageWrapper">
    <div class="profile-container">
      <img class="avatar-image" :src="image">
      <div class="infoBlock">
        <div class="name">
          {{ hero.name }}
        </div>

        <div>Birth Year: {{ hero.birth_year }}</div>
        <div>Height: {{ hero.height }}</div>
        <div v-if="species.name" class="link" @click.prevent="openSpecies">
          Species: {{ species.name }}
        </div>
        <div>Mass: {{ hero.mass }}</div>
        <div>Gender: {{ hero.gender }}</div>
        <div>Hair Color: {{ hero.hair_color }}</div>
        <div>Skin Color: {{ hero.skin_color }}</div>
        <div class="link" @click.prevent="openPlanet">
          Homeworld: {{ planet }}
        </div>
      </div>
    </div>
    <div class="additionalInformationWrapper">
      <div class="blockWrapper">
        <span class="blockTitle">Related Films</span>
        <div class="itemWrapper">
          <div v-for="film in hero.films" :key="film.url">
            <Film :url="film" />
          </div>
        </div>
      </div>
      <div v-show="vehicles" class="blockWrapper">
        <span>Related vehicles</span>
        <div class="itemWrapper">
          <div v-for="vehicle in hero.vehicles" :key="vehicle.url">
            <Vehicle :url="vehicle" />
          </div>
        </div>
      </div>
      <div v-show="starships" class="blockWrapper">
        <span class="blockTitle">Related Starships</span>
        <div class="itemWrapper">
          <div v-for="starship in hero.starships" :key="starship.url">
            <Starships :url="starship" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Film from '~/components/Film'
import Vehicle from '~/components/Vehicle'
import Starships from '~/components/Starship'

export default {
  name: 'IdHero',
  components: {
    Film,
    Vehicle,
    Starships
  },
  data: () => ({
    name: null,
    image: null,
    planet: null,
    planetUrl: null,
    hero: {},
    species: {},
    vehicles: null,
    starships: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const heroImage = await this.$axios.get('https://akabab.github.io/starwars-api/api/id/' + this.$route.params.id + '.json')
    const heroJson = await this.$axios.get('https://swapi.dev/api/people/' + this.$route.params.id)
    if (heroJson.data.species.length > 0) {
      const species = await this.$axios.get(heroJson.data.species)
      this.species = species.data
    }
    const planet = await this.$axios.get(heroJson.data.homeworld)
    this.hero = heroJson.data
    this.image = heroImage.data.image
    this.planet = planet.data.name
    this.planetUrl = planet.data.url
    this.vehicles = this.hero.vehicles.length
    this.starships = this.hero.starships.length
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
.heroPageWrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
}

.additionalInformationWrapper {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
}

.link {
  cursor: pointer;
}

.avatar-image {
  width: 250px;
  height: 300px;
}

.infoBlock {
  color: red;
  padding-left: 20px;
  height: 300px;
}

.profile-container {
  display: flex;
  align-items: center;
  margin-top: 100px;
  border: 1px solid red;
  border-radius: 10px;
  padding: 10px;
  width: 100%;
}

.name {
  font-weight: 700;
  font-size: 25px;
}

.itemWrapper {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.blockWrapper {
  height: 100%;
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
