<template>
    <div class="card">
        <div class="holographic-container">
            <div class="holographic-card">
                <div class="cardHeader">
                    <img class="pokeball" src="../assets/pokeball.png" />
                    <h4>
                        {{ id }} - {{ name }}
                    </h4>
                
                </div>
                <div class="cardBody">
                    <img
                      v-if="sprite"
                      class="cardImage"
                      :src="sprite"
                      loading="lazy"
                    />

                    <div>
                        <div class="cardAbility">
                
                        </div>
                        <div class="cardType">
                            <h3>Type(s): </h3>
                            <p v-for="type in types" :key="type">
                              {{ capitalizeFirstLetter(type) }}
                            </p>

                        </div>
                        <div class="cardDetails">
                            <p class="cardWeight"> Weight: {{ weight }} lbs</p>
                        </div>
                    </div>
                </div>
                <div class="cardFooter">
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import capitalizeFirstLetter from '../utils.ts';

interface PokemonCardProps {
  id?: number;
  name?: string;
  sprite?: string;
  types?: string[];
  abilities?: any[];
  dexEntry?: string;
  weight?: number;
}

const props = defineProps<PokemonCardProps>();
</script>

<style scoped>

.card {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

.holographic-container {
  display: flex;
  justify-content: center;
  align-items: center;
  background: #000;
  border-radius: 20px;
  padding: 10px;
}

.holographic-card {
  width: 250px;
  min-height: 300px;
  background: #111;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
  border-radius: 15px;
  transition: all 0.5s ease;
  padding: 1rem;
  color: white;
  box-shadow: 0 0 8px rgba(0,255,255,0.15);
}

.holographic-card:hover {
  transform: scale(1.05);
  box-shadow: 0 0 20px rgba(0,255,255,0.5);
}

.holographic-card::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(
    0deg,
    transparent,
    transparent 30%,
    rgba(0, 255, 255, 0.3)
  );
  transform: rotate(-45deg);
  transition: all 0.5s ease;
  opacity: 0;
}

.holographic-card:hover::before {
  opacity: 1;
  transform: rotate(-45deg) translateY(100%);
}

.cardHeader {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
  color: var(--color-text);
}

.pokeball {
  height: 24px;
  width: 24px;
}

.cardBody {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 1rem;
}

.cardImage {
  height: 120px;
  width: 120px;
}

.cardType,
.cardDetails {
  text-align: left;
  font-size: 0.9rem;
  color: var(--color-text);
}

.cardType p {
  margin: 0.25rem 0;
}



</style>