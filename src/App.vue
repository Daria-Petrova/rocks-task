<script setup>
import HeaderComponent from "@/components/HeaderComponent.vue";
import FilterComponent from "@/components/FilterComponent.vue";
</script>

<template>
  <header>
    <HeaderComponent/>
  </header>
  <main>
    <h1 class="visually-hidden">Test task for developer</h1>
    <FilterComponent/>
  </main>
</template>

<style>
body {
  background-color: #ecf1f6;
}
</style>
