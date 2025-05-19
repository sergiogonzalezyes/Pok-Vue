<template>
    <div>
        <div v-if="!isLoading" class="cards">
            <PokemonCard v-for="pokemon in pokemonData"
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
    </div>

    <LoadingOverlay v-if="isLoading" />

</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import capitalizeFirstLetter from '../utils.ts'
import PokemonCard from '../components/PokemonCard.vue';
import LoadingOverlay from '../components/LoadingOverlay.vue'

const pokemonList = ref([]);
const pokemonData = ref([]);
const isLoading = ref(false);
const errMsg = "Oops! It appears something has gone wrong. Please try again.";

onMounted(() => {
    loadTwenty();
})

async function loadTwenty() {
    const url = 'https://pokeapi.co/api/v2/pokemon?limit=20&offset=0'
    isLoading.value = true;

    const promiseArray = [];

    try {
        const response = await fetch(url);
        if (!response.ok) {
            throw new Error(`Response status: ${response.status}`)
        }

        const json = await response.json();
        const results = json.results;
        
        results.map((element) => {
            const url = element.url;
            promiseArray.push(loadData(url));
        });

        await new Promise(resolve => setTimeout(resolve, 200));
    
        pokemonData.value = await Promise.all(promiseArray);




    } catch (error) {
        console.error(error.message);
    } finally {
        setTimeout(() => {
            isLoading.value = false;
        }, 1500);

    }

}


async function loadData(url) {
    try {
        const response = await fetch(url);
        // console.log(response)

        if (!response.ok) {
            throw new Error(`Response status: ${response.status}`)
        }

        const json = await response.json();

        const pokemonJson = {
            id: json.id,
            name: capitalizeFirstLetter(json.name),
            abilities: json.abilities,
            species: json.species,
            sprite: json.sprites.front_default,
            stats: json.stats,
            types: json.types,
            weight: json.weight
        }

        return pokemonJson

    } catch (error) {
    console.error(error.message);
    }
}


</script>

<style scoped>

.cards {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    gap: 2rem;
    padding: 20px;
}

</style>