<template>
  <div class="planetWrapper">
    <div class="planetProfile">
      <CommonImage path="planets" />
      <div class="infoBlock">
        <div class="name">
          {{ planet.name }}
        </div>
        <div>Population: {{ planet.population }}</div>
        <div>Сlimate: {{ planet.climate }}</div>
        <div>Diameter: {{ planet.diameter }} km</div>
        <div>Rotation period: {{ planet.rotation_period }} days</div>
        <div>Orbital period: {{ planet.orbital_period }} days</div>
        <div>Surface water: {{ planet.surface_water }}%</div>
      </div>
    </div>
    <div class="additionalInformationWrapper">
      <div class="blockWrapper">
        <span class="blockTitle">Related Films</span>
        <div class="itemWrapper">
          <div v-for="film in planet.films" :key="film.url">
            <Film :url="film" />
          </div>
        </div>
      </div>
      <div class="blockWrapper">
        <span class="blockTitle">Residents</span>
        <div class="itemWrapper">
          <div v-for="resident in planet.residents" :key="resident">
            <Resident :url="resident" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Film from '~/components/Film'
import Resident from '~/components/Resident'
import CommonImage from '~/components/CommonImage'

export default {
  name: 'IdPlanet',
  components: {
    Film,
    Resident,
    CommonImage
  },
  data: () => ({
    planet: {}
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const planet = await this.$axios.get('https://swapi.dev/api/planets/' + this.$route.params.id)
    this.planet = planet.data
    this.$store.commit('loading', false)
  }
}
</script>

<style scoped>
.planetWrapper{
  display: flex;
  flex-direction: column;
  max-width: 100vw;
}
.planetProfile{
  display: flex;
  align-items: center;
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
  align-items: center;
  justify-content: flex-start;
  width: 100%;
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
