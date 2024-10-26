<script setup>

 import { onMounted, ref } from 'vue';
 import PocketBase from 'pocketbase';

const pb = new PocketBase('http://127.0.0.1:8090');
const name = ref("");
const data = ref([])
const audioPlayer=ref(null)
async function submit(){
    
    const record = {
    table1: name.value
    };
    await pb.collection("workshop").create(record);
    data.value.push(record);
    if(audioPlayer.value){
        audioPlayer.value.play()
    }
}

onMounted(async () => {
    const result = await pb.collection("workshop").getFullList();
    data.value = result;
});

</script>

<template>
    <form @submit="(e)=>e.preventDefault()" class="my-5 mx-3">
        <input v-model="name" class="px-3 py-3 rounded shadow-md"/>
        <button @click="submit" class="p-3 bg-amber-500/50 rounded transition hover:bg-amber-200"> Submit </button>
    </form>
    <audio ref="audioPlayer">
        <source src="./assets/amazed-onniich.mp3" type="audio/mpeg">
    </audio>
   
    <ul>
        <li v-for="item in data">
            {{ item.table1 }}
        </li>
    </ul>
    
</template>

<style>
    ul {
        text-align: center;
        font-family: Arial;
        font-weight: bolder;
    }
    /* body {
        background-color: rgb(97, 17, 17);
    }
  */
</style>
