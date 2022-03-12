<template>
  <div>
    <ul class="profilesWrapper">
      <li v-for="hero in heroes" :key="hero.name">
        <div href="#" class="heroName" @click.prevent="openHero(hero)">
          <div class="profileWrapper">
            <img class="avatar-image" :src="hero.image">
            <div>{{ hero.name }}</div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data: () => ({
    heroes: null
  }),
  mounted () {
    this.$store.commit('loading', true)
    this.$axios.get('https://akabab.github.io/starwars-api/api/all.json').then((data) => {
      this.heroes = data.data
      this.$store.commit('loading', false)
    }
    )
  },
  methods: {
    openHero (hero) {
      this.$router.push('/hero/' + hero.id)
    }
  }
}
</script>

<style>
li {
  list-style-type: none;
}

.profilesWrapper {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
}

.profileWrapper {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  width: 100px;
  margin: 10px;
}

.heroName {
  color: red;
  text-decoration: none;
  cursor: pointer;
}

.avatar-image {
  width: 100px;
  height: 120px;
}
</style>
