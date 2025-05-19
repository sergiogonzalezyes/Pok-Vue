<template>
    <div>
        <img src="../assets/pokeball.png" class="rotate" width="200" height="200" />
        <h1> Loading... </h1>
    </div>
</template>

<script setup lang="ts">

</script>

<style scoped>

div {
    height: calc(100vh - 100px);
    width: 100vw;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: fixed;
    z-index: 1001;
    padding: 0;
    margin: 0;
}


.rotate {
  animation: rotation 2s infinite linear;
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