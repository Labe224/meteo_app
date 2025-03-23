<template>
 
    <h1 class="text-center font-bold pt-1.5 text-2xl">Météo du jour </h1>
    <hr class="w-full my-2">
    <div class="flex-col flex w-full sm:flex-row" v-if="meteoData.ville"> 
    <div class="bg-white rounded-lg shadow-lg overflow-hidden w-full max-w-sm mx-4  h-1/1">
        <div class=" bg-blue-600 text-white text-center p-4">
            <h2 class="text-2xl font-bold">Météo de {{ meteoData.ville }}</h2>
            <p class="text-lg">{{ formaterDateDuJour() }}</p>
        </div>
        <div class="flex items-center p-4" :class="isday(meteoData.jour_nuit)">
            <div class="w-24 h-24">
                <img :src="image_correspond(meteoData.nuage)" alt="Icône météo" class="w-full h-full object-cover">
            </div>
            <div class="ml-4">
                <p class="text-4xl font-bold">{{meteoData.temperature}}°C</p>
                <p class="text-lg">{{ meteoData.nuage }}</p>
            </div>
        </div>
        <div class="text-center p-4 bg-blue-100">
            <p>Humidité: {{meteoData.humidity}} %</p>
            <p>Vent: {{ meteoData.wind }} km/h</p>
            <p>Précipitation: {{meteoData.preci}} %</p>
        </div>
    <h4 class="text-center font-bold pt-2"> Prevision horaire</h4>
  <div class="caroussel-container flex overflow-x-auto whitespace-nowrap bg-blue-300 " >
    <div v-if="meteoData.ville" v-for="i in 24" class="caroussel-itmes w-1/3 m-2 rounded-lg shadow-lg overflow-hidden max-w-sm mx-4 py-3 flex-shrink-0" :class="isday(meteoData.jours_nuits[i-1])">
      <p class="text-md font-bold text-center pt-0.5"> {{derniersCaracteres(meteoData.horaire[i-1])}}</p>
      <div class="flex items-center p-4 w-full">
        <div class="">
          <img :src="image_correspond(meteoData.horaire_cloud[i-1])" alt="Icône météo" class="w-full h-full object-cover">
        </div>
        <div class="ml-4">
          <p class="text-md font-bold">{{ meteoData.horaire_temp[i-1] }}°C</p>
        </div>
      </div>
      <p style="font-size: 12px;" class="text-center py-3 font-bold">{{ meteoData.horaire_cloud[i-1]}}</p>
    </div>
    
  </div>
</div>

<div class=" bg-white rounded-xl shadow-md overflow-hidden sm:w-2/3" v-if="meteoData.ville">
  <div class="p-8">
    <h1 class="text-3xl font-bold text-center text-blue-600 mb-6">Info Ville</h1>
    <div class="space-y-4">
      <p class="text-lg text-gray-700"><span class="font-semibold">Nom pays:</span> {{ meteoData.pays }}</p>
      <div class="flex justify-center">
        <img :src="'https://flagsapi.com/'+meteoData.code+'/flat/64.png'" class="w-16 h-16">
      </div>
      <p class="text-lg text-gray-700"><span class="font-semibold">Code pays:</span> {{ meteoData.code }}</p>
      <p class="text-lg text-gray-700"><span class="font-semibold"> Capitale:</span> {{ meteoData.capital }}</p>
      <p class="text-lg text-gray-700"><span class="font-semibold">Langues:</span> {{ meteoData.lang }} </p>
      <p class="text-lg text-gray-700"><span class="font-semibold">Population:</span> {{ meteoData.pop }} hbts</p>
      <p class="text-lg text-gray-700"><span class="font-semibold">Superficie:</span> {{ meteoData.sup }} Km<sup>2</sup></p>
      <p class="text-lg text-gray-700"><span class="font-semibold">Monaie:</span> {{ meteoData.mon }}</p>

      <p class="text-lg text-gray-700"><span class="font-semibold">Region:</span> {{ meteoData.region }}</p>
      <p class="text-lg text-gray-700"><span class="font-semibold">Time Zone:</span> {{ meteoData.time_zone }} GMT</p>
      <p class="text-lg text-gray-700"><span class="font-semibold">Time Zone Name:</span> {{ meteoData.time_zone_name }}</p>
    </div>
  </div>
</div>
</div>
        
</template>


<style scoped>
.bg-image {
    background-image: url(../assets/images/meteo.jpg);
    background-position: center;
}
</style>

<script setup>
const props = defineProps({
  meteoData: {
    type: Object,
    required: true
  }
});
 
function isday(params) {
  if (params==0)
     return "bg-black text-white"
  return "bg-white text-black"
     
  
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



function formaterDateDuJour() {
  // Tableaux pour les noms des jours et des mois
  const jours = ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"];
  const mois = ["Janvier", "Février", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"];

  // Obtenir la date actuelle
  const date = new Date();

  // Récupérer le jour de la semaine, le jour du mois, le mois et l'année
  const jourSemaine = jours[date.getDay()]; // getDay() retourne un indice (0 pour Dimanche, 1 pour Lundi, etc.)
  const jourMois = date.getDate(); // getDate() retourne le jour du mois (1 à 31)
  const moisAnnee = mois[date.getMonth()]; // getMonth() retourne un indice (0 pour Janvier, 1 pour Février, etc.)
  const annee = date.getFullYear(); // getFullYear() retourne l'année (ex: 2025)

  // Retourner la date formatée
  return `${jourSemaine} ${jourMois} ${moisAnnee} ${annee}`;
}

function derniersCaracteres(chaine) {
  // Vérifier si la chaîne a au moins 5 caractères
  if (chaine.length <= 5) {
    return chaine; // Retourne toute la chaîne si elle fait 5 caractères ou moins
  }

  // Retourner les 5 derniers caractères
  return chaine.slice(-5);
}
// Utilisation de la prop dans le composant



</script>