<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
<!-- As a link -->
<nav class="navbar bg-body-tertiary">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">PetSchool</a>
  </div>
  <div class="container-fluid">
    <RouterLink
    class="navbar-brand"
     to="/pets">
    Lista Pets
    </RouterLink>
  </div>
</nav>

</template>

<style scoped>

</style>
