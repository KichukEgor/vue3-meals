<template>
  <div class="min-h-full flex-1 grid grid-rows-[60px,1fr]">
    <navbar/>
    <main class="max-w-[1200px] w-full mx-auto relative">
      <router-view/>
    </main>
  </div>
</template>

<script setup>
import Navbar from "../components/Navbar.vue";
</script>
