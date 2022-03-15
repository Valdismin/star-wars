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
    <ul class="starshipsWrapper">
      <li v-for="starship in starships" :key="starship.name">
        <div href="#" class="starshipName" @click.prevent="openStarship(starship)">
          <div class="starshipWrapper">
            <Starship starship-trigger :url="starship.url" />
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import Starship from '~/components/Starship'

export default {
  name: 'StarshipsPage',
  components: {
    Starship
  },
  data: () => ({
    starships: {},
    url: 'https://swapi.dev/api/starships?page=1',
    nextUrl: null,
    previousUrl: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const starships = await this.$axios.get(this.url)
    this.starships = starships.data.results
    this.nextUrl = starships.data.next
    this.$store.commit('loading', false)
  },
  methods: {
    openStarship (starship) {
      const starshipId = starship.url.split('/')[5]
      this.$router.push('/starship/' + starshipId)
    },
    async pushNextUrl (url) {
      this.$store.commit('loading', true)
      this.url = url
      const starships = await this.$axios.get(this.url)
      this.starships = starships.data.results
      this.nextUrl = starships.data.next
      this.previousUrl = starships.data.previous
      this.$store.commit('loading', false)
    },
    async pushPreviousUrl (url) {
      this.$store.commit('loading', true)
      this.url = url
      const starships = await this.$axios.get(this.url)
      this.starships = starships.data.results
      this.nextUrl = starships.data.next
      this.previousUrl = starships.data.previous
      this.$store.commit('loading', false)
    }
  }
}
</script>

<style>
li {
  list-style-type: none;
}

.starshipsWrapper {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}

.starshipWrapper {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  margin: 10px;
}

.starshipName {
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
