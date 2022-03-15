<template>
  <div class="resident" @click.prevent="openResident">
    <img class="image" :src="image">
    <div class="title">
      {{ resident.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilmComponent',
  // eslint-disable-next-line vue/require-default-prop
  props: { url: String },
  data: () => ({
    resident: {},
    image: null,
    heroId: null
  }),
  async mounted () {
    this.$store.commit('loading', true)
    const resident = await this.$axios.get(this.url)
    this.resident = resident.data
    this.heroId = this.resident.url.split('/')[5]
    const heroImage = await this.$axios.get('https://akabab.github.io/starwars-api/api/id/' + this.heroId + '.json')
    this.image = heroImage.data.image
    this.$store.commit('loading', false)
  },
  methods: {
    openResident () {
      this.$router.push('/hero/' + this.heroId)
    }
  }
}
</script>

<style scoped>
.resident {
  width: 120px;
  padding: 10px;
  cursor: pointer;
}

.image {
  width: 100px;
  height: 130px;
}

.title {
  color: red;
  font-size: 14px;
  font-weight: 500;
}
</style>
