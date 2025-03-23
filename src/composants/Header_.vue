<template>
  <header class="flex flex-col pb-2 bg-blue-100">
    <div class="bg-blue-800 p-1 mb-2 shadow-2xl flex w-full justify-between">
        <h1 class="text-white">Word_metéo</h1>
        <div class="flex justify-start align-center mb-1.5 gap-10 pr-10">
        <a href="#" class="text-red-300 font-semibold" @click="envoyer(1)">Accueil</a>
        <a href="#" class="text-gray-300 font-semibold" @click="envoyer(0)">Météo du monde </a>
        </div>
    </div>

  <div class="flex gap-2 justify-center">
      <input type="text" placeholder="Entrez le nom d'une localité" class="w-3/4 border-1 text-center border-black rounded-2xl" v-model="ville">
      <button><i class="" @click="clique">Search</i></button>

  </div>
</header>

    
</template>

<script setup>
import { ref } from 'vue';

const emits=defineEmits(["nom_ville" ,"menu"])

const ville= ref('')
 function clique(){
     emits("nom_ville",ville.value)
 }

function envoyer(params) {
    emits("menu",params)
}
</script>