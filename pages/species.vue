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
    <ul class="speciesWrapper">
      <li v-for="race in species" :key="race.name">
        <div href="#" class="raceName" @click.prevent="openRace(race)">
          <div class="raceWrapper">
            <Race :url="race.url" />
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import Race from '~/components/Race'

export default {
  name: 'RacePage',
  components: {
    Race
  },
  data: () => ({
    species: null,
    url: 'https://swapi.dev/api/species?page=1',
    nextUrl: null,
    previousUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const species = await this.$axios.get(this.url)
    this.species = species.data.results
    this.nextUrl = species.data.next
    this.$store.commit('loading', false)
  },
  methods: {
    openRace (race) {
      const raceId = race.url.split('/')[5]
      this.$router.push('/race/' + raceId)
    },
    async pushNextUrl (url) {
      this.$store.commit('loading', true)
      this.url = url
      const species = await this.$axios.get(this.url)
      this.species = species.data.results
      this.nextUrl = species.data.next
      this.previousUrl = species.data.previous
      this.$store.commit('loading', false)
    },
    async pushPreviousUrl (url) {
      this.$store.commit('loading', true)
      this.url = url
      const species = await this.$axios.get(this.url)
      this.species = species.data.results
      this.nextUrl = species.data.next
      this.previousUrl = species.data.previous
      this.$store.commit('loading', false)
    }
  }
}
</script>

<style scoped>
li {
  list-style-type: none;
}

.speciesWrapper {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}

.raceWrapper {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  margin: 10px;
}

.raceName {
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
