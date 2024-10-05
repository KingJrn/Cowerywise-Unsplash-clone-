<script setup>
// Importing components
import Search from './components/base/Search.vue';
import Card from './components/UI/Card.vue';
import Modal from './components/UI/Modal.vue';

// Importing Vue functions
import { ref} from 'vue'

// API configuration
const BASE_URL = 'https://api.unsplash.com';
const END_POINT = '/search/photos';
const ACCESS_KEY = 'yOrq5HCj-fW7QSHU1p1PvaLhEvd9Tp_b_QgV7F7IauA';

// Reactive references
const loading = ref(false);
const AfricanPhotos = ref([]);
const searchParam = ref('');
const showModal = ref(false);
const photoPreviewId = ref(null);

// Function to search photos from Unsplash API
async function searchPhotos(query, page = 1, perPage = 8) {
  const endpoint = `${BASE_URL}${END_POINT}?query=${query}&page=${page}&per_page=${perPage}`;
  try {
    const response = await fetch(endpoint, {
      headers: {
        'Authorization': `Client-ID ${ACCESS_KEY}`
      }
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    console.log('Search results:', data.results);
    return data.results;
  } catch (error) {
    console.error('Error searching photos:', error);
  }
}

// Function to display photos
async function displayPhotos(param) {
  loading.value = true;
  let query = searchParam.value === '' ? 'African' : searchParam.value;
  try {
    const photos = await searchPhotos(query);
    if (param === 'search') {
      AfricanPhotos.value = [];
    }
    photos.forEach(photo => {
      AfricanPhotos.value.push({
        id: photo.id,
        url: photo.urls.small,
        urlRegular: photo.urls.regular,
        author: photo.user.name,
        location: photo.user.location
      });
      console.log(photo.urls.small);
      console.log(AfricanPhotos.value);
    });
  } catch (error) {
    console.error('Error displaying photos:', error);
  } finally {
    loading.value = false;
  }
}

// Initial call to display photos
displayPhotos();

// Function to preview a photo in a modal
function previewPhoto(id) {
  photoPreviewId.value = id;
  showModal.value = true;
}

// Function to close the modal
function closeModal() {
  showModal.value = false;
}
</script>

<template>
  <header class="center-items">
    <!-- Search component -->
    <Search class="search-container" v-model="searchParam" @change="displayPhotos('search')" />
    <h2 v-if="searchParam">
      <span class="_search">Search Results for <span class="query"> "{{ searchParam }}"</span></span>
    </h2>
  </header>

  <main>
    <!-- Card container displaying photos -->
    <div class="card-container">
      <Card v-for="photo in AfricanPhotos" :key="photo.id" :image="photo.url" :location="photo.location" :id="photo.id"
        :name="photo.author" :url="photo.url" @click="previewPhoto(photo.id)" :loading="loading" />
    </div>

    <!-- Modal for photo preview -->
    <div v-show="showModal" class="modal-container">
      <a href="javascript:void(0)" class="cancel" @click="closeModal">x</a>

      <div class="modal-wrapper">
        <Modal :photos="AfricanPhotos" :id="photoPreviewId" />
      </div>
    </div>
  </main>
</template>

<style scoped>
/* Styles for the cancel button in the modal */
.cancel {
  position: absolute;
  right: 20%;

  top: 0px;
  color: var(--cw-white);
  z-index: 1000;
  text-decoration: none;
  font-size: xx-large;
  font-weight: 200;
}

/* Styles for the modal container */
.modal-container {
  position: fixed;
  z-index: 1000;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
}

/* Styles for the modal wrapper */
.modal-wrapper {
  width: 80%;
  max-width: 600px;
  height: 50vh;
  margin: auto;
  position: relative;
  z-index: 1000;
  margin-top: 10vh;
}

/* Styles for the header */
header {
  background-color: var(--cw-light-gray);
  height: 300px;
  width: 100%;
}

/* Styles for centering items */
.center-items {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 30px;
}

/* Styles for the search container */
.search-container {
  width: 90%;
  max-width: 600px;
}

/* Styles for the card container */
.card-container {
  width: 90%;
  max-width: 1200px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
  margin: auto;
  padding: 1rem;
  justify-items: center;
  align-items: center;
  position: relative;
  top: -70px;
}

/* Styles for search result text */
._search {
  color: var(--cw-navy-blue);
}

.query {
  color: var(--cw-navy-blue-light);
}

@media (max-width: 767px) {
  .cancel {
  right: 10%;
  top: 50px;
  }
}
/* Responsive styles for card container */
@media (max-width: 600px) {
  .card-container {
    grid-template-columns: 1fr;
  }
}

@media (min-width: 601px) and (max-width: 900px) {
  .card-container {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 901px) {
  .card-container {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>
