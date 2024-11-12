<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <header>
    <div class="wrapper">
      <nav class="flex">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/paiement">Exercice paiement</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>
