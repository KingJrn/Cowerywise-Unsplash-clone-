<script setup>
import { computed } from 'vue'

const props = defineProps({
  photos: {
    type: Array,
    required: true
  },
  id: {
    type: String,
    required: true
  }
})

const preview = computed(() => {
  const photo = props?.photos.find(photo => photo.id === props.id)
  if (photo) {
    return {
      url: photo.urlRegular,
      author: photo.author,
      location: photo.location
    }
  } else {
    return {
      url: '',
      author: '',
      location: ''
    }
  }
})
</script>

<template>
  <div class="card" v-if="preview.url">
    <img :src="preview.url" :alt="preview.author">
    <div class="card-body">
      <h2>{{ preview.author }}</h2>
      <p>{{ preview.location }}</p>
    </div>
  </div>
  <div v-else>
    No image found.
  </div>
</template>
<style scoped>
.card {
  background-color: var( --cw-white);
  color: black;
  border-radius: 8px;
  box-shadow: 0 4px 5px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  margin: auto;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 90%;
  height: 50%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px;
}

.card-body {
  background-color: rgba(255, 255, 255, 0.8);
  width: 100%;
  padding: 10px; 
}

.card img {
  width: 100%;
  height: calc(100% - 50px); 
  object-fit: cover;
}

/* Media Queries for Responsiveness */
@media (max-width: 576px) {
  .card {
    width: 95%;
    height: 60%;
    padding: 5px;
  }

  .card-body {
    padding: 5px;
  }

  .card img {
    height: calc(100% - 40px);
  }
}

@media (min-width: 400px) and (max-width: 768px) { 
  .card {
    width: 80%;
    height: 90%;
    padding: 8px;
  }

  .card-body {
    padding: 8px;
  }

  .card img {
    height: calc(100% - 45px);
  }
}

@media (min-width: 768px) {
  .card {
    width: 50%;
    height: 90%;
    padding-bottom: 3rem;
  }
}
</style>
