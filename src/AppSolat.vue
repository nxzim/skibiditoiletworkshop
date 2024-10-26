<script setup>

import {ref, onMounted} from 'vue'

let prayerTimes = ref([])

onMounted(async function(){

    let response = await fetch('https://www.e-solat.gov.my/index.php?r=esolatApi/TakwimSolat&period=today&zone=WLY01')
    let data = await response.json()
    prayerTimes.value = data.prayerTime 
    console.log(data.prayerTime)

})

</script>

<template>
    
    <table>
        <thead>
            <tr>

                <th>Hijri</th>
                <th>Date</th>
                <th>Day</th>
                <th>Imsak</th>
                <th>Fajr</th>
                <th>Dhuhr</th>
                <th>Asr</th>
                <th>Maghrib</th>
                <th>Isha</th>


            </tr>
        </thead>
        <tbody>
            <tr v-for="prayerTime in prayerTimes">
                <th>{{  prayerTime.hijri }}</th>
                <th>{{  prayerTime.date }}</th>
                <th>{{  prayerTime.day }}</th>
                <th>{{  prayerTime.imsak }}</th>
                <th>{{  prayerTime.fajr }}</th>
                <th>{{  prayerTime.syuruk }}</th>
                <th>{{  prayerTime.dhuhr }}</th>
                <th>{{  prayerTime.asr }}</th>
                <th>{{  prayerTime.maghrib }}</th>
                <th>{{  prayerTime.isha }}</th>
            </tr>
        </tbody>
    </table>
 
</template>

<style>

</style>
