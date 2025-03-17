<script setup>

import { ref } from "vue";
import axios from "axios";
import { useMessage } from 'naive-ui'
import { defineComponent } from 'vue'

const pokemons = ref([]);


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
  <h2>Liste des Pokémons</h2>
  <div class="flex-container">
    <n-card v-for="pokemon in pokemons" :key="pokemon.id">
      <h2>{{pokemon.name}}</h2>
      <div class="type-pv-container">
        <h3 class="pv">{{ pokemon.lifePoints }} PV</h3>
        <n-tag
          size="medium"
          round
          :style="{ backgroundColor: typeColors[pokemon.type.name] || 'gray', color: 'black' }"
        >
          {{ pokemon.type.name }}
        </n-tag>
      </div>
      <template #cover>
        <img :src="pokemon.imageUrl" />
      </template>
      <p>{{ pokemon.height }}m | {{ pokemon.weight }}kg</p>
      <n-tag class="pv"
          size="medium"
          round>
        {{ pokemon.attack.name }} ->  -{{ pokemon.attack.damages }} PV
      </n-tag>
    </n-card>
  </div>
</template>

<style scoped>

.flex-container 
{
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.pv
{
  color:red;
  font-weight: bold;
}

.type-pv-container {
  display: flex;
  align-items: center;
  gap: 30px;
}

.n-card 
{
  max-width: 200px;
}
</style>
