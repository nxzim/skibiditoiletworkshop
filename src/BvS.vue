
<script setup>

import {ref} from 'vue'
let count = ref(0)
let msg="Batman v Superman"
let word= ref("")
let villains = ref(["joker","penguin","adudu"])
function Increment(){
  count.value +=3
  console.log(count.value)
  console.log(input.value)
}
function AddVillain(){

}

</script>

<template>
 
 <h1 style="font-family:Impact;font-size: xx-large;">{{ msg }}</h1>
  <div>
    <a href="https://www.youtube.com" target="_blank">
      <img src="https://1000logos.net/wp-content/uploads/2021/10/Batman-Logo.png" class="logo" alt="Vite logo"/>
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="https://1000logos.net/wp-content/uploads/2017/06/Superman-Logo.png" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <p>{{ count }}</p>
  <button @click="Increment">Vote</button> <br><br>
  <input v-model="word"/> <br><br>
  <button class="bg-red-900">Add villains</button>
  <ul>
    
  </ul>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #710606);
  width: 300px;
  height: 300px;
  transform: rotate(360deg);
  transition: width 2s, height 2s, transform 2s;
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #164e83);
}
</style>
