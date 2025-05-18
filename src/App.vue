<script setup>
import { RouterView, RouterLink } from 'vue-router'
</script>

<template>
  <header>
    <nav>
      <img id="mew" src="/mew.png" />
      <RouterLink to="/">Poké-Vue</RouterLink>
    </nav>
  </header>

  <main>
    <RouterView />
  </main>
</template>

<style scoped>

main {
  padding: 100px 0 0 0;
}

header {
  width: 100%;
  height: 100px;
  padding: 1rem;
  background-color: var(--color-background-soft);
  position: fixed;
  top: 0;
  z-index: 1000;
  /* border-bottom: 1px solid var(--color-border); */
}

nav {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-size: 1rem;
}

nav a.router-link-exact-active {
  font-weight: bold;
  color: var(--color-text);
}

#mew {
  height: 70px;
  width: 70px;
}

</style>
