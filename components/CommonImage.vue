<template>
  <div>
    <img class="image" :src="imageUrl">
  </div>
</template>

<script>
export default {
  name: 'CommonImage',
  props: { path: String },
  data: () => ({
    imageUrl: null
  }),
  mounted () {
    let result
    try {
      result = require(`~/assets/img/${this.path}/${this.$route.params.id}.jpg`)
    } catch (e) {
      if (e.code !== 'MODULE_NOT_FOUND') {
        throw e
      }
      result = require('~/assets/img/placeholder.jpg')
    }
    this.imageUrl = result
  }
}
</script>

<style scoped>
.image {
  width: 300px;
  height: 300px;
}
</style>
