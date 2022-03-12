<template>
  <div class="homeWrapper">
    <NuxtLink to="/heroes" class="mainContainer heroes">
      <div class="textBackground">
        Heroes
      </div>
    </NuxtLink>
    <NuxtLink to="/planets" class="mainContainer planets">
      <div class="textBackground">
        Planets
      </div>
    </NuxtLink>
    <NuxtLink to="/films" class="mainContainer films">
      <div class="textBackground">
        Films
      </div>
    </NuxtLink>
    <NuxtLink to="/starships" class="mainContainer starships">
      <div class="textBackground">
        Starships
      </div>
    </NuxtLink>
    <NuxtLink to="/vehicles" class="mainContainer vehicles">
      <div class="textBackground">
        Vehicles
      </div>
    </NuxtLink>
    <NuxtLink to="/species" class="mainContainer species">
      <div class="textBackground">
        Species
      </div>
    </NuxtLink>
  </div>
</template>

<script>
export default {
  name: 'IndexComponent'
}
</script>

<style scoped>
.homeWrapper {
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  padding-top: 200px;
}

.textBackground{
  width: 100%;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(0,0,0,.4);
  color: red;
  font-size: 20px;
  font-weight: 700;
}

.mainContainer {
  height: 200px;
  width: 400px;
  background-repeat: no-repeat;
  background-size: contain;
  color: white;
  text-decoration: none;
  margin: 10px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  padding-bottom: 40px;
}

.heroes {
  background-image: url("~/star-wars/assets/img/categories/character.jpg");
}

.planets {
  background-image: url("~/star-wars/assets/img/categories/planets.jpg");
}

.films {
  background-image: url("~/star-wars/assets/img/categories/films.jpg");
}

.starships {
  background-image: url("~/star-wars/assets/img/categories/starships.jpg");
}

.vehicles {
  background-image: url("~/star-wars/assets/img/categories/vehicles.jpg");
}

.species {
  background-image: url("~/star-wars/assets/img/categories/species.jpg");
}
</style>
