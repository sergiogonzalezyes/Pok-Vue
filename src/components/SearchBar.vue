<template>
  <div class="search-wrapper">
    <img class="rotate" width="40" height="40" src="../assets/pokeball.png" alt="pokeball" />
    <input
      type="text"
      v-model="searchTerm"
      placeholder="Search Pokémon..."
      class="search-input"
    />
    <button class="search-btn" @click="searchPokemon(searchTerm)">
      Search
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { lowercaseAllLetter } from '../utils.ts'

interface PokemonEntry {
  name: string;
  url: string;
}

const searchTerm = ref<string>('');
const pokemonNames = ref<PokemonEntry[]>([]);

onMounted(() => {
    loadAll();

})

async function loadAll() {
  const url = `https://pokeapi.co/api/v2/pokemon?limit=1300`;

  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    const results: { name: string, url: string }[] = json.results;
    pokemonNames.value = results.map((n: any) => ({name: n.name, url: n.url}));

  } catch (error: unknown) {
    if (error instanceof Error) {
      console.error(error.message);
    } else {
      console.error('Unknown error occurred.');
    }
  } 
}

function searchPokemon(searchTerm: string) {
    loadSearch(searchTerm);
}

function loadSearch(searchTerm: string) {
    const formattedSearchTerm = lowercaseAllLetter(searchTerm);
    const searchResult = pokemonNames.value.filter((e: PokemonEntry) => e.name.includes(formattedSearchTerm));
    
    // NOTE: CONTINUE HERE WITH LOOPING THROUGH AND USING ASYNC AWAIT TO ENSURE ALL REQUESTS ARE MADE FOR MATCHING NAMES

}



</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=VT323&display=swap');

.search-wrapper {
  display: flex;
  align-items: center;
  background-color: #d8d8c0;
  border: 2px solid #222;
  box-shadow: inset -2px -2px 0 #fff, inset 2px 2px 0 #888, 2px 2px 0 #000;
  border-radius: 6px;
  font-family: 'VT323', monospace;
  max-width: 420px;
  height: 62px;
  margin: 0 auto;
  padding: 4px 8px;
  gap: 8px;
}

.search-wrapper img.rotate {
  width: 36px;
  height: 36px;
  flex-shrink: 0;
}

.search-input {
  flex-grow: 1;
  background-color: #f4f4ec;
  border: none;
  outline: none;
  font-family: 'VT323', monospace;
  font-size: 1.7rem;
  padding: 2px 6px;
  color: #333;
}

.search-btn {
  background-color: #7bf360;
  border: 1px solid #222;
  box-shadow: inset -1px -1px 0 #000, inset 1px 1px 0 #fff;
  border-radius: 4px;
  font-family: 'VT323', monospace;
  font-size: 2rem;
  padding: 2px 8px;
  margin-left: 4px;
  cursor: pointer;
  color: #000;
}

.search-btn:active {
  transform: scale(0.96);
  box-shadow: inset 1px 1px 2px #222;
}

@media (max-width: 600px) {
  .search-wrapper {
    max-width: 100%;
    width: 100%;
    height: 50px;
    padding: 2px 4px;
    gap: 4px;
    transform: scale(1); /* Don't scale down if you're resizing directly */
  }

  .search-wrapper img.rotate {
    width: 24px;
    height: 24px;
  }

  .search-input {
    font-size: 1rem;
    padding: 2px 4px;
  }

  .search-btn {
    font-size: 1.2rem;
    padding: 2px 4px;
  }
}

.rotate {
  animation: rotation 5s infinite linear;
}

@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}

</style>
