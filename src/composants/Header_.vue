<template>
  <header class="flex flex-col pb-2 bg-blue-100">
    <div class="bg-blue-800 p-1 mb-2 shadow-2xl flex w-full justify-between">
        <h1 class="text-white">Word_metéo</h1>
        <div class="flex justify-start align-center mb-1.5 gap-10 pr-10 max-md:hidden">
        <a href="#" class="text-red-300 font-semibold" @click="envoyer(1,0)">Accueil</a>
        <a href="#" class="text-gray-300 font-semibold" @click="envoyer(0,0)">Météo du monde </a>
        </div>
        <div class=" md:hidden"><i class="fas fa-bars text-xl text-white mr-2" @click="swit"></i></div>
        <div class="absolute top-8 bg-blue-400 flex flex-col gap-2 m-2 right-1.5 p-2 hidden" id="swit">
        <a href="#" class="text-white font-semibold" @click="envoyer(1,1)">Accueil</a>
        <a href="#" class="text-white font-semibold" @click="envoyer(0,1)">Météo du monde </a>
      </div> 
    </div>
  <form @submit.prevent="clique">
  <div class="flex gap-2 justify-center">
    
      <input type="text" placeholder="Entrez le nom d'une localité" class="w-3/4 border-1 text-center border-black rounded-2xl" v-model="ville">
      <button class="border-1 bg-blue-100 border-r-2 px-1 py-0.5"><i class="fa-solid fa-search" @click="clique" ></i></button>
    
  </div>
</form>
</header>

    
</template>

<script setup>
import { ref } from 'vue';

const emits=defineEmits(["nom_ville" ,"menu"])

function swit(){
    document.getElementById("swit").classList.toggle("hidden")
}
const ville= ref('')
 function clique(){
    if (ville.value==""){
      alert("vous n'avez pas saisi de ville")
    }
     emits("nom_ville",ville.value)
     ville.value=''
 }

function envoyer(params,pars2) {
    emits("menu",params)
    
    if(pars2==1)
    swit()
}
</script>