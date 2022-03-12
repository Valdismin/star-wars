<template>
  <div>
    <ul class="profilesWrapper">
      <li v-for="planet in planets" :key="planet.name">
        <div href="#" class="planetName" @click.prevent="openPlanet(planet)">
          <div class="profileWrapper">
            <div>{{ planet.name }}</div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'PlanetsPage',
  data: () => ({
    planets: null
  }),
  async mounted () {
    const planets = await this.$axios.get('https://swapi.dev/api/planets')
    this.planets = planets.data.results
  },
  methods: {
    openPlanet (planet) {
      const planetId = planet.url.split('/')[5]
      this.$router.push('/planet/' + planetId)
    }
  }
}
</script>

<style>
li{
  list-style-type: none;
}
.profilesWrapper{
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}
.profileWrapper{
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  width: 100px;
  margin: 10px;
}
.planetName{
  color: red;
  text-decoration: none;
  cursor: pointer;
}
.avatar-image{
  width: 100px;
  height: 120px;
}
</style>
