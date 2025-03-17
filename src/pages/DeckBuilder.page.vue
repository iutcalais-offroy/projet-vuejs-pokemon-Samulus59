<script setup>

import { ref, computed, onMounted } from "vue";
import axios from "axios";
import { useMessage } from 'naive-ui'
import { defineComponent } from 'vue'

const pokemons = ref([]);
const recherche = ref("");

const RecupererPokemons = async () => 
{
  try 
  {
    const response = await axios.get("https://pokemon-api-seyrinian-production.up.railway.app/pokemon-cards");

    console.log("Récupération des pokémons réussie", response.data);
    pokemons.value = response.data;
  } 
  catch (error) 
  {
    console.error("Erreur lors de la récupération:", error);
  }
};

onMounted(() => {RecupererPokemons()});

const filteredPokemons = computed(() => {
  return pokemons.value.filter(pokemon =>
    pokemon.name.toLowerCase().includes(recherche.value.toLowerCase())
  );
});

const typeColors = {
  Fire: "coral",
  Water: "lightblue",
  Grass: "lightgreen",
  Electric: "yellow",
  Psychic: "purple",
  Ice: "cyan",
  Fighting: "orange",
  Poison: "violet",
  Ground: "brown",
  Flying: "lightblue",
  Bug: "lime",
  Rock: "gray",
  Ghost: "darkviolet",
  Dragon: "darkblue",
  Dark: "black",
  Steel: "silver",
  Fairy: "pink",
  Normal: "lightgray",
};
</script>

<template>
  <div class="recherche">
    <input v-model:="recherche" round placeholder="Rechercher par nom" />
  </div>
  <br><br>
  <div class="flex-container">
    <n-card v-for="pokemon in filteredPokemons" :key="pokemon.id" hoverable>
      <h2>{{pokemon.name}}</h2>
      <div class="type-pv-container">
        <h3 class="pv">{{ pokemon.lifePoints }} PV</h3>
        <n-tag
          size="medium"
          round
          :style="{ backgroundColor: typeColors[pokemon.type.name] || 'gray', color: 'black' }">
          {{ pokemon.type.name }}
        </n-tag>
      </div>
      <template #cover>
        <img :src="pokemon.imageUrl" />
      </template>
      <p class="attributs">{{ pokemon.height }}m | {{ pokemon.weight }}kg</p>
      <n-tag class="pv"
          size="medium"
          round>
        {{ pokemon.attack.name }} ->  -{{ pokemon.attack.damages }} PV
      </n-tag>
      <div class="boutton_ajout">+</div>
    </n-card>
  </div>
</template>

<style scoped>

.flex-container 
{
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

h2
{
  line-height:0;
}

.pv
{
  color:red;
  font-weight: bold;
  line-height: 0;
}

.attributs
{
  text-align: center;
}

.type-pv-container {
  display:flex;
  justify-content: center;
  text-align: center;
  align-items: center;
  gap: 20px;
}

.n-card 
{
  max-width: 200px;
  background-color: rgb(231, 231, 231);
  text-align: center;
  border: solid 2px black;
  transition: all 0.3s ease-in-out;
  position: relative;
  overflow: hidden;
}

.n-card:hover
{
  background-color: rgb(170, 169, 169);
  max-width: 250px;
  transition: all 0.3s ease-in-out;
  box-shadow: 5px 5px 5px rgba(0, 0, 0, 1);
}

.boutton_ajout {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 50px;
  height: 50px;
  background-color: lightgreen;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  font-weight: bold;
  color: black;
  cursor: pointer;
  opacity: 0;
  transform: translate(-50%, -50%) scale(0.8);
  transition: opacity 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.n-card:hover .boutton_ajout {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1);
}

input 
{
  width: 300px;
  text-align: center;
  margin: auto;
  padding: 8px;
  font-size: 16px;
  border: 2px solid black;
  border-radius: 5px;
  outline: none;
}

.recherche {
  display: flex;
  justify-content: center; 
  align-items: center;     
  width: 100%;             
  margin: 10px;  
}

</style>