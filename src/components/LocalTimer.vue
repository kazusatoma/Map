<template>
    <footer>
        <span class="timer" v-if="props.searchedList[0]" style="text-align:center;">
            Latest Searched Location : {{ props.searchedList[props.searchedList.length - 1].Location }} Time Zone :
            {{ localTimeZone }} Time: {{ localTime }}
        </span>
        <span class="timer" v-else>No search result yet</span>
    </footer>
</template>

<script setup>
import { ref, watch, defineProps } from 'vue';
import axios from 'axios'
import ezlocalTime from 'ez-local-time'
import { google_api_key } from './api/apikey';
const localTime = ref()
const localTimeZone = ref()
const props = defineProps({
    searchedList: Array
})
let myTimer

watch(props.searchedList, () => {
    if (props.searchedList[props.searchedList.length - 1]) {
        let lastLocationLat = props.searchedList[props.searchedList.length - 1].Coordinates.lat
        let LastLocationLng = props.searchedList[props.searchedList.length - 1].Coordinates.lng
        apiRequest(lastLocationLat, LastLocationLng)
    }
    else alert('error')
})

const apiRequest = (lat, lng) => {
    axios.get(`https://maps.googleapis.com/maps/api/timezone/json?location=${lat}%2C${lng}&timestamp=1331161200&key=${google_api_key}`).then((res) => {
        if (res.data.status === 'OK') {
            localTimeZone.value = res.data.timeZoneId
        }
        else {
            alert(res.data.status)
        }
    })
}

watch(localTimeZone, () => {
    clearInterval(myTimer)
    myTimer = setInterval(() => {
        localTime.value = ezlocalTime(localTimeZone.value).time
    }, 1000)
})

</script>

<style scoped>
footer {
    margin: 30px;
    text-align: center;
}
</style>