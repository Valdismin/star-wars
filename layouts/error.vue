<template>
  <section>
    <h1>Some error occurred</h1>
    <nuxt-link class="link" to="/">
      Home
    </nuxt-link>
  </section>
</template>

<script>
export default {
  name: 'ErrorPage'
}
</script>

<style scoped>
section {
  width: 100%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.link{
  color: red;
  border: 1px solid red;
  border-radius: 10px;
  padding: 10px;
}
h1 {
  color: red;
  padding-bottom: 20px;
}
</style>
