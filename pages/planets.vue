<template>
  <div>
    <div class="paginatorWrapper">
      <div v-show="previousUrl !== null" class="button" @click.prevent="pushPreviousUrl(previousUrl)">
        Previous
      </div>
      <div v-show="nextUrl !== null" class="button" @click.prevent="pushNextUrl(nextUrl)">
        Next
      </div>
    </div>
    <ul class="planetsWrapper">
      <li v-for="planet in planets" :key="planet.name">
        <div href="#" class="planetName" @click.prevent="openPlanet(planet)">
          <div class="planetWrapper">
            <Planet :url="planet.url" />
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
    planets: null,
    url: 'https://swapi.dev/api/planets?page=1',
    nextUrl: null,
    previousUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const planets = await this.$axios.get(this.url)
    this.planets = planets.data.results
    this.nextUrl = planets.data.next
    this.$store.commit('loading', false)
  },
  methods: {
    openPlanet (planet) {
      const planetId = planet.url.split('/')[5]
      this.$router.push('/planet/' + planetId)
    },
    async pushNextUrl (url) {
      this.$store.commit('loading', true)
      this.url = url
      const planets = await this.$axios.get(this.url)
      this.planets = planets.data.results
      this.nextUrl = planets.data.next
      this.previousUrl = planets.data.previous
      this.$store.commit('loading', false)
    },
    async pushPreviousUrl (url) {
      this.$store.commit('loading', true)
      this.url = url
      const planets = await this.$axios.get(this.url)
      this.planets = planets.data.results
      this.nextUrl = planets.data.next
      this.previousUrl = planets.data.previous
      this.$store.commit('loading', false)
    }
  }
}
</script>

<style scoped>
li {
  list-style-type: none;
}

.planetsWrapper {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}

.planetWrapper {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  margin: 10px;
}

.planetName {
  color: red;
  text-decoration: none;
  cursor: pointer;
  font-size: 20px;
  font-weight: 600;
}

.button{
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  height: 30px;
  color: red;
  margin-right: 15px;
  border: 1px solid red;
  border-radius: 10px;
  padding: 10px;
}
.paginatorWrapper{
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin: 50px 0;
}
</style>
