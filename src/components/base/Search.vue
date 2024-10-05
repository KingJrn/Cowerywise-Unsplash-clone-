<script setup>
import { defineModel } from 'vue'

import search from '../../assets/images/search.svg';

const searchInput = defineModel()
</script>

<template>
  <div class="bg search-wrapper">
    <img :src="search" alt="search icon" width="25px" class="search-icon">
    <input type="search" class="" placeholder="search for photo" v-model="searchInput">
  </div>
</template>
<style scoped>
.search-wrapper {
  position: relative;
  padding-inline: 20px;
}

.search-wrapper input {
  width: 100%;
  padding-block: 12px;
  border-radius: 8px;
  border: 1px solid white;
  box-shadow: 0.5px 0.5px white;
  padding-left: 60px;
  font-size: 16px;
}

input::placeholder,
input {
  color: var(--cw-navy-blue) !important;
}

input:focus,
input:active,
input:focus-visible,
input:hover {
  border: 1px solid gray !important;
}

.search-icon {
  position: absolute;
  top: 25%;
  left: 40px;
}

@media (max-width: 768px) {
  .search-wrapper {
    padding-inline: 10px;
  }

  .search-wrapper input {
    padding-left: 50px;
    font-size: 14px;
  }

  .search-icon {
    left: 30px;
  }
}

@media (max-width: 480px) {
  .search-wrapper {
    padding-inline: 5px;
  }

  .search-wrapper input {
    padding-left: 40px;
    font-size: 12px;
  }

  .search-icon {
    left: 20px;
  }
}
</style>
