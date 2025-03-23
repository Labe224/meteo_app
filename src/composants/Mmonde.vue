<template>
    <h1 class="text-center font-bold py-3 text-2xl">Météo des grandes villes du monde</h1>
    <div class="flex">
    <div class="flex flex-col w-1/2 gap-2"> 
    <div class="w-full bg-gray-500 flex justify-between"  v-for="i in largestCities" >
      <div class="meteo pl-4 flex w-1/2">
        <div class=" w-2/3"> 
          <h1>{{ i }}</h1>
          <p>13 °</p>
          <p>Eclairci </p>
        </div>
          <div class="w-10 h-10">
                <img src="../assets/images/soleil.png" alt="Icône météo" class="w-full h-full object-cover">
            </div>
          

      </div>
     <div class="info-ville w-1/2 bg-gray-200 pl-4 tewt-black">
         <p>Nom pays</p>
         <p>Région</p>

     </div>
    </div>
    </div>
  
    <div class="bg-blue-300 ml-4 w-1/2 h-[200px]">
        <h2 class="text-center pb-4 font-bold">Ajouter une nouvelle ville</h2>
        <form action="" class="flex flex-col gap-2 pl-3">
           <div><label for="">Nom de la ville:</label> <input type="text" class="border-black border-b-1 w-2/3"></div>
            <button>Ajouter</button>
        </form>
    </div>

</div>
</template>

<script setup>
const largestCities = [
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
    "New York City",
    "Karachi",
    "Chongqing",
    "Istanbul",
    "Buenos Aires"
];

</script>