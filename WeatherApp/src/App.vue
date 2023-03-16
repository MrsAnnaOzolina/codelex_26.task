<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="header">
      <nav>
        <div >
          <RouterLink to="/"><span class="header__link">Home</span></RouterLink>
          <RouterLink to="/about"><span class="header__link">About</span></RouterLink>
        </div>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
.header{
  display: flex;
  justify-content: center;
}
.header__link{
  color: white;
  margin: 30px;
  font-size: 1.2em;
}</style>
