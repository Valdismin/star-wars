<template>
  <div>
    <Navbar />
    <main class="mainClass">
      <div class="container gridClass background">
        <div />
        <div v-if="loading" class="background">
          <LoadingBar />
        </div>
        <Nuxt />
        <div />
      </div>
    </main>
  </div>
</template>

<script>
import Navbar from '~/components/Navbar'
import LoadingBar from '~/components/LoadingBar'

export default {
  name: 'DefaultLayout',
  components: {
    Navbar, LoadingBar
  },
  computed: {
    loading () {
      return this.$store.state.loading
    }
  }
}
</script>

<style scoped>
.container{
  min-height: 100vh;
}
.background{
  background-image: url("~/star-wars/static/background.jpeg");
  background-repeat: no-repeat;
  background-size: cover;
}
.gridClass {
  max-width: 100%;
  display: grid;
  grid-template-columns: 1fr 5fr 1fr;
  padding: 0;
}
.mainClass{
  min-height: 100vh;
}
</style>
