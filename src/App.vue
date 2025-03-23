<template>
  <Header @nom_ville=" main_current" @menu="menuaf"></Header>
  <Body :meteo-data="meteo_data" v-if="af"></Body> 
  <Mmonde v-if="!af" :donnees="donnees_grand_ville" @affihe="afficher"></Mmonde>
  
  
</template>
<script setup>
import Body from './composants/Body_.vue'
import Header from './composants/Header_.vue'
import Mmonde from './composants/Mmonde.vue'

import { ref } from 'vue'

const meteo_data=ref({})
let af= ref(1)

function menuaf(params) {
  af.value=params
  
}


async function get_localisation_reverse(params) {   // fonction qui permet de recuperer la géolocatlisation de la ville et 
// retourner sa longitude et sa latitude
   var url="https://api.geoapify.com/v1/geocode/reverse?"
    let url_params= new URLSearchParams({ 
        lat:params.lat,
        lon:params.long,
        apiKey:"e8421c9d7f2c43be998a5a0e70c63d8c"
    })

  let response= await  fetch(url+url_params)
  
  if (response.ok){
    let data = await response.json()
    return data.features[0].properties.city
  }
}

async function get_geolocation(params) {   // fonction qui permet de recuperer la géolocatlisation de la ville et 
// retourner sa longitude et sa latitude
   var url="https://api.geoapify.com/v1/geocode/search?"
    let url_params= new URLSearchParams({ 
        text:params,
        apiKey:"e8421c9d7f2c43be998a5a0e70c63d8c"
    })

  let response= await  fetch(url+url_params)
  
  if (response.ok){
    let data = await response.json()
    let long =data.features[0].properties.lon
    let lat = data.features[0].properties.lat
    let coun_code=data.features[0].properties.country_code
    coun_code=coun_code.toUpperCase()
    let coun=data.features[0].properties.country
    let region= data.features[0].properties.state
    let time_zone=data.features[0].properties.timezone.offset_DST
    let time_zone_name=data.features[0].properties.timezone.name
     return {lat,long,coun_code,coun,region,time_zone,time_zone_name}
    
  }
    return {"erreur":"erreur"}
}

async function get_meteo(data) {  // fonction qui permet de recuperer les données de la météo
    var url="https://api.open-meteo.com/v1/forecast?"
    let url_params= new URLSearchParams({ 
        latitude:data.lat,
        longitude:data.long,
        current:"temperature_2m,wind_speed_10m,relative_humidity_2m,precipitation_probability,cloud_cover,is_day",
        hourly:"temperature_2m,wind_speed_10m,relative_humidity_2m,precipitation_probability,cloud_cover,is_day"

    })
  console.log(data.long, data.lat)
  let response= await  fetch(url+url_params)
  
  if (response.ok){
    let data = await response.json()
    let nuage=data.current.cloud_cover
    let appre=""
    if (nuage < 30) {
  appre = "Eclairci";
} else if (nuage < 60) {
  appre = "Partiellement nuageux";
} else {
  appre = "Nuageux";
}

let appreci= []
let jours_nuits=[]

for (let i=0; i <24 ;i++){
jours_nuits.push(data.hourly.is_day[i])
    
}


for (let i=0; i <24 ;i++){
  let appre=""
    if (data.hourly.cloud_cover[i] < 30) {
  appre = "Eclairci";
} else if (data.hourly.cloud_cover[i]< 60) {
  appre = "Partiellement nuageux";
} else {
  appre = "Nuageux";
}
appreci.push(appre)
    
}

       
    let objet={
        "temperature":data.current.temperature_2m,
        "wind": data.current.wind_speed_10m,
        "humidity":data.current.relative_humidity_2m,
        "preci":data.current.precipitation_probability,
        "nuage": appre,
        "horaire":data.hourly.time,
        "horaire_temp":data.hourly.temperature_2m,
        "horaire_cloud":appreci,
        "jour_nuit": data.current.is_day,
        "jours_nuits": jours_nuits
          }
    return objet
  }

    
}
async function main_current(params,v) { // fonction appelé à chaque recherche de ville 
    menuaf(1)
    
    let data = await get_geolocation(params)
    let donnees=await get_meteo(data)

    
    donnees.code=data.coun_code
    console.log(donnees.pays)
    let donnee2=await getCountryInfo(donnees.code)

    donnees.ville=params
    if(v==1)
      remplis_donnes(donnees,donnee2,data)
    else
     meteo_data.value=remplis_donnes(donnees, donnee2,data)
    
   return donnees
}
function remplis_donnes(donnees, donnee2,data) {
   
    donnees.region=data.region
    donnees.time_zone=data.time_zone
    donnees.time_zone_name=data.time_zone_name
    donnees.pop=donnee2.pop
    donnees.capital=donnee2.capital
    donnees.sup=donnee2.sup
    donnees.mon=donnee2.mon
    donnees.pays=data.coun
    donnees.jour_nuit=donnee2.jour_nuit
    donnees.jours_nuits=donnees.jours_nuits
    donnees.lang=donnee2.lang
    return donnees
}
async function depart(dat) { // fonction appelé à chaque recherche de ville 
    let params= await get_localisation_reverse(dat)
    let data = await get_geolocation(params)
    let donnees=await get_meteo(data)
    donnees.code=data.coun_code
    let donnee2=await getCountryInfo(donnees.code)
    donnees.ville=params
    meteo_data.value=remplis_donnes(donnees, donnee2,data,0)

  
}


async function getCountryInfo(countryName) {
  try {
    const response = await fetch(`https://restcountries.com/v3.1/alpha/${countryName}`);
    console.log(`https://restcountries.com/v3.1/name/${countryName}`)
    const data = await response.json();
    
    if (data.status === 404) {
      console.error("Pays non trouvé");
      return;
    }

    const country = data[0];
    let capital= country.capital[0]
    let pop= country.population
    let sup=country.area
    let lang= Object.values(country.languages).join(", ")
    let mon=Object.values(country.currencies)[0].name

    return {capital,pop,sup,lang,mon}
  } catch (error) {
    console.error("Erreur lors de la récupération des données :", error);
  }
}

   // Fonction qui permet de recuperer les coordonnées de l'utilisateurs 

   function getLocation() {
    return new Promise((resolve, reject) => {
    // Vérifier si le navigateur supporte l'API Geolocation
    if (navigator.geolocation) {
      // Demander la position de l'utilisateur
      navigator.geolocation.getCurrentPosition(
        (position) => {
          // Succès : résoudre la promesse avec la latitude
          resolve(
            {'lat':position.coords.latitude,
            'long':position.coords.longitude
        }),
        (error) => {
          // Erreur : rejeter la promesse avec le message d'erreur
          reject("Erreur lors de la récupération de la position : " + error.message);
        }
    });
    } else {
      // Le navigateur ne supporte pas l'API Geolocation
      reject("Geolocation n'est pas supporté par ce navigateur.");
    }
  });
}
getLocation()
.then( (data)  => {
  depart(data)
})


// fonction qui permet de charger la météo des grandes villes de du monde 
const donnees_grand_ville=ref([])
const liste_ville = [
    "Tokyo",
    "Delhi",
    "Shanghai",
    "São Paulo",
    "Mexico City",
    "Cairo",
    "Dhaka",
    "Mumbai",
    "Beijing",
    "Osaka",
];
async function ville_monde(){
   for ( let i=0; i<liste_ville.length; i++){
    console.log(liste_ville[i])
    let donnees= await main_current(liste_ville[i],1)
    donnees.nom=liste_ville[i]
    donnees_grand_ville.value.push(donnees)
   }
 console.log(donnees_grand_ville.value)
}
ville_monde()

// fonction qui permet de charger les données 
async function afficher(params){
  console.log(params)
    await main_current(params,0)
    menuaf(1)
}
</script>