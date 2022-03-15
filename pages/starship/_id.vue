<template>
  <div>
    <div class="starshipProfile">
      <CommonImage path="starships" />
      <div class="infoBlock">
        <div class="name">
          {{ starship.name }}
        </div>
        <div>Class: {{ starship.starship_class }}</div>
        <div>Passengers: {{ starship.passengers }}</div>
        <div>Cargo capacity: {{ starship.cargo_capacity }}</div>
        <div>Hyperdrive rating: {{ starship.hyperdrive_rating }}</div>
        <div>Passengers: {{ starship.passengers }}</div>
        <div>Max speed: {{ starship.max_atmosphering_speed }}</div>
        <div>Length: {{ starship.length }}</div>
      </div>
    </div>
    <div class="additionalInformationWrapper">
      <div v-show="films" class="blockWrapper">
        <span class="blockTitle">Films</span>
        <div class="itemWrapper">
          <div v-for="film in starship.films" :key="film">
            <Film :url="film" />
          </div>
        </div>
      </div>
      <div v-show="pilots" class="blockWrapper">
        <span class="blockTitle">Pilots</span>
        <div class="itemWrapper">
          <div v-for="pilot in starship.pilots" :key="pilot">
            <Resident :url="pilot" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import Resident from '~/components/Resident'
import Film from '~/components/Film'
import CommonImage from '~/components/CommonImage'

export default {
  name: 'IdStarship',
  components: {
    Resident,
    Film,
    CommonImage
  },
  data: () => ({
    starship: {},
    pilots: null,
    films: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const starship = await this.$axios.get('https://swapi.dev/api/starships/' + this.$route.params.id)
    this.starship = starship.data
    this.pilots = this.starship.pilots.length
    this.films = this.starship.films.length
    this.$store.commit('loading', false)
  }
}
</script>

<style scoped>
.starshipProfile{
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
