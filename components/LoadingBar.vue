<template>
  <div class="loading-page">
    <div class="loading" />
  </div>
</template>
<script>

export default {
  name: 'LoadingBar'
}
</script>
<style scoped>
.loading-page {
  position: fixed;
  width: 100vw;
  height: 100%;
  top: 80px;
  left: 0;
  z-index: 1000;
  padding: 1rem;
  text-align: center;
  font-size: 3rem;
  font-family: sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-image: url("~/star-wars/static/background.jpeg");
  background-repeat: no-repeat;
  background-size: cover;
}

.loading {
  display: inline-block;
  width: 100px;
  height: 100px;
  border: 10px solid darkred;
  border-radius: 50%;
  border-top-color: red;
  animation: spin 0.5s ease-in-out infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
