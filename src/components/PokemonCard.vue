<template>

  <div class="pokemon-frame">
    <div class="pokedex-card">
      <span class="frame-dot top-left"></span>
      <span class="frame-dot top-right"></span>
      <span class="frame-dot bottom-left"></span>
      <span class="frame-dot bottom-right"></span>
      <div class="button-panel">
        <span class="gba-button green">PAGE</span>
        <span class="gba-button green">AREA</span>
        <span class="gba-button green">CRY</span>
        <span class="gba-button green">SIZE</span>
        <span class="gba-button red">CANCEL</span>
      </div>
      <span class="power-led"></span>

      <!-- Top: Sprite + Info -->
      <div class="top-section">
        <div class="sprite-box">
          <img v-if="sprite" :src="sprite" alt="pokemon sprite" />
        </div>
        <div class="info-box">
          <p class="id-name">No.{{ id }} {{ name }}</p>
          <p class="species">{{ species || 'Species Unknown' }}</p>
          <p class="types"> {{ types }} </p>
        </div>
      </div>
      <!-- Mid: HT / WT -->
      <div class="measurements">
        <div class="row"><strong>HT</strong> {{ formattedHeight }}</div>
        <div class="row"><strong>WT</strong> {{ formattedWeight }}</div>
      </div>
      <!-- Bottom: Dex Entry -->
      <div class="entry-section">
        <div class="entry-header">PAGE 1</div>
        <div class="entry-body">
          {{ formattedFlavorText || 'This Pokémon’s data is currently unavailable.' }}
        </div>
      </div>
    </div>
  </div>
</template>


<script setup lang="ts">
import { computed } from 'vue';

interface PokemonCardProps {
  id?: number;
  name?: string;
  sprite?: string;
  types?: string[];
  abilities?: any[];
  dexEntry?: string;
  weight?: number;
  height?: number;
  species?: string;
  flavor_text?: string;
}

const props = defineProps<PokemonCardProps>();

// Convert height (decimeters) to feet/inches
const formattedHeight = computed(() => {
  if (!props.height) return '—';
  const inches = Math.round(props.height * 3.937);
  const feet = Math.floor(inches / 12);
  const remainingInches = inches % 12;
  return `${feet}′${remainingInches}″`;
});

// Convert weight (hectograms) to pounds
const formattedWeight = computed(() => {
  if (!props.weight) return '—';
  const pounds = (props.weight * 0.2205).toFixed(1);
  return `${pounds} lbs.`;
});

const formattedFlavorText = props.flavor_text?.replace(/[\u000c\f\n\r\t]+/g, ' ')

</script>






<style scoped>

@import url('https://fonts.googleapis.com/css2?family=VT323&display=swap');

.pokedex-frame {
  background-color: #444;
  padding: 0.5rem;
  border-radius: 10px;
  box-shadow: inset 0 0 0 3px #222, 4px 4px 0 #111;
  display: inline-block;
}



.pokedex-card {
  width: 320px;
  height: 360px;
  background-color: #f8f9fa;
  border: 2px solid #333;
  font-family: 'VT323', monospace;
  font-size: 1rem;
  color: #222;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding: 0.75rem;
  box-shadow: 2px 2px 0 #aaa;
  image-rendering: pixelated;
  font-smooth: never;
  -webkit-font-smoothing: none;
  border-radius: 6px;
  overflow: hidden;
  background-color: #e3e3e3;
  padding: 1rem;
  border: 2px solid #444;
  border-radius: 6px;
  box-shadow:
    inset -1px -1px 0 #aaa, /* Inner bevel light */
    inset 1px 1px 0 #666,   /* Inner bevel shadow */
    3px 3px 0 #1a1a1a;      /* Hard drop shadow */
  background-image: repeating-linear-gradient(
    to bottom,
    #d2d697,
    #f8f9fa 2px,
    #eeeaea 3px
  );
  position: relative; /* 🔧 Required for both glass shine and screw dots */
  overflow: hidden;
  animation: boot-in 0.4s ease-in;
}

@keyframes boot-in {
  0% {
    transform: scale(0.9);
    opacity: 0;
    filter: brightness(150%);
  }
  100% {
    transform: scale(1);
    opacity: 1;
    filter: brightness(100%);
  }
}

.pokedex-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 30%;
  background: linear-gradient(to bottom, rgba(255, 255, 255, 0.2), transparent);
  z-index: 1;
  pointer-events: none;
}

.gba-button:active {
  transform: scale(0.96);
  box-shadow: inset 1px 1px 2px #222;
}

/* Top section: sprite + info */
.top-section {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.sprite-box {
  background-color: #eee;
  border: 1px solid #aaa;
  padding: 4px;
  height: 96px;
  width: 96px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.sprite-box img {
  width: 80px;
  image-rendering: pixelated;
}

.info-box {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  text-align: right;
  gap: 0.25rem;
}

.id-name,
.entry-header {
  text-shadow: 1px 1px 0 #000;
}


.species {
  font-size: 0.9rem;
  color: #555;
}

/* Measurements */
.measurements {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  padding: 0.25rem 0.5rem;
  background-color: #f8f8f8;
  border: 1px solid #ccc;
}

.measurements .row {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
}

/* Dex Entry */
.entry-section {
  border: 1px solid #888;
  background-color: #fff;
  box-shadow: 0 4px 0 #d48dfb; /* 👈 This adds the GBA-style purple edge */
}


.entry-header {
  background-color: #2d6cdf;
  color: white;
  font-weight: bold;
  padding: 0.25rem 0.5rem;
  font-size: 0.85rem;
  border-bottom: 1px solid #222;
  text-shadow: 1px 1px 0 #000, 0 0 4px #cbe3ff;
}

.entry-body {
  height: 75px;
  padding: 0.5rem;
  font-size: 0.9rem;
  line-height: 1.4;
  color: #333;
  background-color: #fafafa;
}


.button-panel {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 0.5rem;
}

.gba-button {
  font-family: 'VT323', monospace;
  padding: 2px 6px;
  font-size: 0.75rem;
  border-radius: 4px;
  border: 1px solid #000;
  box-shadow: inset -1px -1px 0 #000, inset 1px 1px 0 #fff;
  text-shadow: 1px 1px 0 #000;
  cursor: pointer;
}

.gba-button.green {
  background-color: #7bf360;
  color: #000;
}

.gba-button.red {
  background-color: #f87070;
  color: #000;
}

.frame-dot {
  position: absolute;
  width: 6px;
  height: 6px;
  background-color: #222;
  border-radius: 50%;
  z-index: 2;
}

.top-left {
  top: 6px;
  left: 6px;
}

.top-right {
  top: 6px;
  right: 6px;
}

.bottom-left {
  bottom: 6px;
  left: 6px;
}

.bottom-right {
  bottom: 6px;
  right: 6px;
}

.power-led {
  position: absolute;
  top: 22px;
  left: 285px;
  width: 13px;
  height: 13px;
  background-color: rgb(255, 0, 0);
  border: 2.1px solid #4b4b4b;
  border-radius: 50%;
  box-shadow: 23px 23px 23px rgb(212, 0, 255);
  z-index: 10;
  animation: pulse-led 1.2s ease-in-out infinite;
}

@keyframes pulse-led {
  0%, 100% {
    box-shadow: 0 0 4px rgb(149, 0, 255);
  }
  50% {
    box-shadow: 0 0 30px rgb(225, 1, 255);
  }
}



</style>