<template>
    <div class="wrapper">
            <button @click="$emit('previous')" v-if="currentPage > 1"><<</button>
            <h3>Page {{ currentPage }}</h3>
            <button @click="$emit('next')" v-if="currentPage < totalPages">>></button>
    </div>
</template>

<script setup lang="ts">
interface Props {
  currentPage: number;
  totalPages: number;
}

const props = defineProps<Props>();

defineEmits<{
  (e: 'next'): void;
  (e: 'previous'): void;
}>();
</script>

<style scoped>

.wrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  height: 100%;
  margin: 0;
}

h3 {
  font-size: 1rem;
  color: var(--color-text);
}

button {
  margin: 0 0.5rem;
  border: none;
  border-radius: 6px;
  background-color: var(--color-background-soft);
  color: var(--color-text);
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.2s ease, transform 0.2s ease;
}

button:hover {
  background-color: var(--color-border-hover);
  transform: translateY(-1px);
}

button:disabled {
  background-color: #ccc;
  color: #666;
  cursor: not-allowed;
  transform: none;
}


</style>