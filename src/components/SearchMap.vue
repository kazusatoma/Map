<template>
  <div style="text-align: center;">
    <a-input class="searchBar" @keyup.enter="handleSearch" v-model:value="searching" placeholder="Enter a location" />
    <a-button @click="handleSearch">Search</a-button>
  </div>
  <div class="searchMap">
    <GoogleMap v-if="center" :api-key="google_api_key" style="width: 80%; height: 40vh"
      :center="center" :zoom="15">
      <MarkerCluster>
        <Marker v-for="(location, i) in searchedListCoord" :options="{ position: location }" :key="i" />
      </MarkerCluster>
    </GoogleMap>
    <div v-else style="width: 80%; height: 40vh"><img src="https://geology.com/world/world-map.gif" /></div>
  </div>
</template>


<script setup>
import { ref, defineProps } from 'vue';
import { GoogleMap, Marker, MarkerCluster } from "vue3-google-map";
import { nanoid } from 'nanoid'
import axios from 'axios'
import { computed } from '@vue/reactivity';
import { message } from 'ant-design-vue';
import { google_api_key } from './api/apikey';
const searching = ref('')
const center = ref()
const searchedListCoord = computed(() => {
  return props.searchedList.map((item) => {
    return item['Coordinates']
  })
})
const props = defineProps({
  addList: Function,
  searchedList: Array
})

//Handle search
const handleSearch = () => {
  if(searching.value === ''){
    message.warning("Location cannot be blank")
  }
  //get coordinates by user entered location name from google geocode API
  axios.get(`https://maps.googleapis.com/maps/api/geocode/json?address=${searching.value}&key=${google_api_key}`)
    .then((res) => {
      if (res.data.results[0]) {
        //Change map center based on the response of google geocode API
        center.value = res.data.results[0].geometry.location
        //Add this search to searched list
        let LocationName = res.data.results[0].formatted_address
        let LocationCoordinate = res.data.results[0].geometry.location
        let id = nanoid()
        let LocationObj = { 'Location': LocationName, 'Coordinates': LocationCoordinate, 'key': id }
        //Callback function which brings data back to App
        props.addList(LocationObj)
        //empty the input box
        searching.value = ''
      }
      else{
        message.warning("Cannot find this location")
      }
    })
}
</script>

<style scoped>
.searchMap {
  justify-content: center;
  display: flex;
  margin: 20px;
}



.searchBar {
  width: min-content;
  margin: 20px;
}

img {
  max-width: 100%;
  min-height: 100%;
}
</style>