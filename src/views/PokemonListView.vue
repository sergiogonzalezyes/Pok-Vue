<template>
  <div>
    <div v-if="!isLoading" class="cards">
      <PokemonCard
        v-for="pokemon in pokemonData"
        :key="pokemon.id"
        :id="pokemon.id"
        :name="pokemon.name"
        :sprite="pokemon.sprite"
        :types="pokemon.types"
        :weight="pokemon.weight"
        :abilities="pokemon.abilities"
        :species="pokemon.species"
        :stats="pokemon.stats"
      />
    </div>

    <div v-if="!isLoading" class="pagination">
      <Pagination
        :currentPage="currentPage"
        :totalPages="totalPages"
        @next="handleNext"
        @previous="handlePrevious"
      />
    </div>

    <LoadingOverlay v-if="isLoading" />
  </div>
</template>



<script setup lang="ts">
import { ref, onMounted } from 'vue';
import capitalizeFirstLetter from '../utils.ts';
import PokemonCard from '../components/PokemonCard.vue';
import Pagination from '../components/PaginationControls.vue';
import LoadingOverlay from '../components/LoadingOverlay.vue';

interface Pokemon {
  id: number;
  name: string;
  sprite: string;
  types: string[];
  weight: number;
  abilities: string[];
  species: any; // You can refine this later
  stats: any;   // Same here
}

const pokemonData = ref<Pokemon[]>([]);
const currentPage = ref(1);
const totalPages = ref(65);
const limit = 20;
const isLoading = ref(false);
const errMsg = "Oops! It appears something has gone wrong. Please try again.";

async function handleNext() {
  currentPage.value = currentPage.value + 1;
  await loadTwenty(currentPage.value);
}

async function handlePrevious() {
  currentPage.value = currentPage.value - 1;
  await loadTwenty(currentPage.value);
}

onMounted(() => {
  loadTwenty(currentPage.value);
});

async function loadTwenty(currentPage: number) {
  const offset = (currentPage - 1) * limit;
  const url = `https://pokeapi.co/api/v2/pokemon?limit=20&offset=${offset}`;
  isLoading.value = true;

  try {
    pokemonData.value = [];

    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    const results: { url: string }[] = json.results;

    const promiseArray = results.map((element) => loadData(element.url));
    const resolvedPokemon = await Promise.all(promiseArray);

    await new Promise((resolve) => setTimeout(resolve, 200));
    pokemonData.value = resolvedPokemon.filter(Boolean) as Pokemon[];
    console.log("Resolved Pokémon array:", resolvedPokemon);


  } catch (error: unknown) {
    if (error instanceof Error) {
      console.error(error.message);
    } else {
      console.error('Unknown error occurred.');
    }
  } finally {
    setTimeout(() => {
      isLoading.value = false;
    }, 1500);
  }
}

async function loadData(url: string): Promise<Pokemon | undefined> {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();

    const pokemonJson: Pokemon = {
      id: json.id,
      name: capitalizeFirstLetter(json.name),
      abilities: json.abilities,
      species: json.species,
      sprite: json.sprites.front_default,
      stats: json.stats,
      types: json.types.map((t: any) => t.type.name), // just extract names if needed
      weight: json.weight
    };

    return pokemonJson;

  } catch (error: unknown) {
    if (error instanceof Error) {
      console.error(error.message);
    }
  }
}
</script>


<style scoped>
.cards {
  overflow-y: auto;
  max-height: calc(100vh - 160px);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 2rem;
  padding: 20px;
  padding-bottom: 100px;
}

.pagination {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  z-index: 100;
}
</style>

