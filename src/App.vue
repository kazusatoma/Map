<template>
  <div class="container">
    <GeoLocator></GeoLocator>
    <SearchMap :addList="addList" :searchedList="searchedList"></SearchMap>
    <SearchHistory :searchedList="searchedList" :delList="delList"></SearchHistory>
    <LocalTimer :searchedList="searchedList"></LocalTimer>
  </div>
</template>

<script setup>
import GeoLocator from './components/GeoLocator.vue';
import SearchMap from './components/SearchMap.vue';
import SearchHistory from './components/SearchHistory.vue';
import LocalTimer from './components/LocalTimer.vue'
import { ref } from 'vue';

const searchedList = ref([])

const addList = (item) => {
  searchedList.value.push(item)
}

const delList = (obj) => {
  for (let key in obj) {
    searchedList.value = searchedList.value.filter((item) => {
      return !(item.key === obj[key])
    })
  }
}
</script>

<style>
html,
body {
  min-height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 0 auto;
  max-width: 1000px;
  height: 100%
}

.container {
  min-height: 100%;
  background-color: rgb(215, 219, 221);
}
</style>
