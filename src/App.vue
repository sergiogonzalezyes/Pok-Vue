<template>
  <header>
    <nav>
      <div class="site-header">
        <img id="mew" src="/mew.png" />
        <RouterLink to="/">Poké-Vue</RouterLink>
      </div>
      <div class="search-wrapper">
        <SearchBar />
      </div>
    </nav>
  </header>

  <main>
    <RouterView />
  </main>
</template>


<script setup>
import { RouterView, RouterLink } from 'vue-router'
import SearchBar from './components/SearchBar.vue'
</script>



<style scoped>

main {
  padding: 120px 0 0 0; /* was 100px */
}

header {
  width: 100%;
  height: 100px;
  padding: 1rem 2rem;
  position: fixed;
  top: 0;
  z-index: 1000;
  background-color: #1f1f1f;
  box-shadow: 0 4px 0 #000;
}

nav {
  display: flex;
  flex-wrap: wrap;              /* 👉 allow children to wrap */
  align-items: center;
  justify-content: space-between;
  gap: 0.5rem;
}

nav a.router-link-exact-active {
  font-weight: bold;
  color: #ffffff;
  font-family: 'VT323', monospace;
  font-size: 1.4rem;
  text-shadow: 1px 1px #000;
}

#mew {
  height: 60px;
  width: 60px;
}

.site-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.site-header a {
  font-size: 1.6rem;
  text-decoration: none;
  color: #fff;
  font-family: 'VT323', monospace;
  text-shadow: 1px 1px #000;
}


@media (max-width: 600px) {


  main {
    padding: 200px 0 0 0; /* was 100px */
  }


  header {
    width: 100%;
    height: 200px;
    padding: 1rem 2rem;
    position: fixed;
    top: 0;
    z-index: 1000;
    background-color: #1f1f1f;
    box-shadow: 0 4px 0 #000;
  }



  nav {
    flex-direction: column;
    align-items: center;
  }

  .site-header {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 0.5rem;
  }
  .search-wrapper {
    justify-content: center;
  }

}



</style>
