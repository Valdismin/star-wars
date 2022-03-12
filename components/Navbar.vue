<template>
  <nav class="customNavbar">
    <ul class="navLinksWrapper">
      <img src="../static/starWarsLogo.png" class="logo" />
      <li class="nav-item">
        <nuxt-link
          exact
          class="navLink"
          no-prefetch
          active-class="active"
          aria-current="page"
          to="/"
        >
          Home
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link class="navLink" active-class="active" to="/heroes">
          Heroes
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link class="navLink" active-class="active" to="/planets">
          Planets
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link class="navLink" active-class="active" to="/films">
          Films
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link class="navLink" active-class="active" to="/starships ">
          Starships
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link class="navLink" active-class="active" to="/vehicles  ">
          Vehicles
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link class="navLink" active-class="active" to="/species  ">
          Species
        </nuxt-link>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'NavbarComponent',
  methods: {}
}
</script>

<style scoped>
.customNavbar {
  width: 100%;
  height: 80px;
  background-color: black !important;
}

.navLinksWrapper {
  height: 100%;
  display: flex;
  align-items: center;
}

.navLink {
  color: red;
  width: 100px;
  height: 100%;
  padding: 10px;
  text-decoration: none;
  font-size: 20px;
  font-weight: 600;
  border: 1px solid red;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-item{
  margin-right: 10px;
  height: 45px;
}
.active{
  color: brown;
  font-size: 22px;
  border: 1px solid brown;
}

.logo{
  height: 70px;
  width: 120px;
  margin-right: 50px;
}
</style>
