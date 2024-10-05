<script setup>
const props = defineProps({
  location: String,
  id: Number,
  image: String,
  name: String,
  url: String,
  loading: Boolean


})
</script>
<template>
  <div v-if="props && !loading" class="card overlay">
    <figure>
      <img :src="props?.url" :alt="props?.name">
    </figure>
    <div class="card-body">
      <h1>{{ props?.name }}</h1>
      <p>{{ props?.location }}</p>
    </div>
  </div>
  <div v-else class="card skeleton-loader skeleton ">
    <div class="card-body description">
      <h1 class=""></h1>
      <p class=" "></p>
    </div>
  </div>
</template>
<style scoped>
.overlay::after {
  position: absolute;
  content: '';
  top: 0;
  left: 0;
  background-color: var(--cw-black);
  width: 100%;
  height: 100%;
  opacity: .3;
}

.card {
  background-color: var(--cw-white);
  color: var(--cw-white);
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  margin: auto;
  height: 350px;
  position: relative;
  display: grid;
}

.skeleton {
  animation: skeleton-loading 1s linear infinite alternate;
}

@keyframes skeleton-loading {
  0% {
    background-color: hsl(200, 20%, 80%);
  }

  100% {
    background-color: hsl(200, 20%, 95%);
  }
}

.skeleton-loader {
  width: 250px;
  background-color: hsl(200, 20%, 80%)
}

.description {
  width: 80%;
}

.description h1 {
  height: 26px;
  margin-bottom: 10px;
  background-color: var(--cw-light-gray);
}

.description p {
  height: 18px;
  background-color: var(--cw-light-gray);
  width: 60%;
}

.skeleton-loader img {
  height: 100%;
  object-fit: cover;
}

.card-body {
  padding: 30px 20px;
  position: absolute;
  bottom: 0;
  z-index: 10;

}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-content {
  padding: 1rem;
}

.card h3 {
  margin: 0 0 0.5rem 0;
}

.card p {
  margin: 0;
}

</style>
