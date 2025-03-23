<template>
    <h1 class="text-center font-bold pt-3 text-2xl max-sm: text-[20px]">Météo des grandes villes du monde</h1>
     <hr class="w-full my-2">
    <div class="flex max-sm:flex-col">
    <div class="flex flex-col w-1/2 gap-2 max-sm:w-full"> 
    <div class="w-full bg-gray-500 flex justify-between"  v-for="i in donnees" >
      <div class="meteo pl-4 flex w-1/2">
        <div class=" w-2/3"  @click="afficher(i.nom)"> 
          <h1>{{ i.nom }}</h1>
          <p>{{ i.temperature }}</p>
          <p>{{ i.nuage }} </p>
        </div>
          <div class="w-10 h-10">
                <img :src="image_correspond(i.nuage)" alt="Icône météo" class="w-full h-full object-cover">
            </div>
          

      </div>
     <div class="info-ville w-1/2 bg-gray-200 pl-4 tewt-black">
         <p><span class="font-bold text-[14px]">Pays:</span>  {{i.pays }}</p>
         <p> <span class="font-bold text-[14px]">Langues</span>: {{ i.lang }}</p>

     </div>
    </div>
    </div>
  
    <div class="bg-blue-300 ml-4 w-1/2 h-[200px] max-sm:mt-3 max-sm:w-full max-sm:ml-0">
        <h2 class="text-center pb-4 font-bold">Ajouter une nouvelle ville</h2>
        <form action="" class="flex flex-col gap-2 pl-3">
           <div><label for="">Nom de la ville:</label> <input type="text" class="border-black border-b-1 w-2/3"></div>
            <button>Ajouter</button>
        </form>
    </div>

</div>
</template>

<script setup>

const props = defineProps({
  donnees: {
    type: Object,
    required: true
  }
});
const emits = defineEmits(["affihe"])
function afficher(params) {
  console.log(params)
  emits("affihe",params)
  
}

function image_correspond(temps) {
  let imag = new URL( '/src/assets/images/default.png', import.meta.url); // Image par défaut au cas où aucune condition ne correspond
  
  if (temps == "Eclairci") {
    const imageUrl = new URL('/src/assets/images/soleil.png', import.meta.url)
     return imageUrl
  } else if (temps == "Nuageux") {
    const imageUrl = new URL('/src/assets/images/nuageux.png', import.meta.url)
    return imageUrl
  } else if (temps == "Partiellement nuageux") {
    const imageUrl = new URL('/src/assets/images/partiel.avif', import.meta.url)
    return imageUrl
  }

  return imag;
}

</script>