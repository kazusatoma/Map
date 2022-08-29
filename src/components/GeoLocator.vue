<template>
    <div style="padding: 20px; margin-left:10% ;">
        <a-button style="margin-right: 15px;" @click="getGeoLocation">Primary Button</a-button>
        <span v-show="coords">Your current location is : {{ coords.latitude }},{{ coords.longitude }}</span>
    </div>
</template>


<script setup>
/*eslint-disable no-undef*/
import { ref,onUnmounted} from 'vue';
const coords = ref({ latitude: 0, longitude: 0 })
const isSupported = 'navigator' in window && 'geolocation' in navigator

//Handle user location acquisition
let watcher = null
const getGeoLocation = (() => {
    if (isSupported) {
        watcher = navigator.geolocation.watchPosition(
            position => (coords.value = position.coords)
        )
    }
    else{
        alert("Cannot acquire your location from your browser")
    }
})
//clear watch before component unmount
onUnmounted(() => {
    if (watcher) navigator.geolocation.clearWatch(watcher)
})
</script>