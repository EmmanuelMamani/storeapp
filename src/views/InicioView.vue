<script setup>
import { RouterLink } from 'vue-router'
</script>
<template>
  <div class="h-screen">
    <div class="bg-blue-600 p-3 h-1/3 relative" id="by-pry" style="border-radius: 0 0 150px 0;">
      <span class="text-white text-2xl font-semibold block w-5/6 mx-auto">Store UP!</span>
      <h1 class="text-white block w-5/6 mx-auto">Software de inventarios</h1>
      <div class="left-7 absolute bottom-0  transform  translate-y-1/2">
        <img src="../components/icons/caja.svg" alt="" class="z-10">
      </div>
    </div>
    <div class="p-3 mt-6">
        <RouterLink to="/login" class="block p-3 my-3 bg-blue-600 text-white w-4/5 mx-auto font-medium text-center rounded-3xl">Iniciar Sesion</RouterLink>
        <RouterLink to="/logup" class="block  p-2  text-blue-600 w-4/5 mx-auto font-medium text-center rounded-3xl border-blue-600 border-2">Registrate</RouterLink>
    </div>
  </div>
</template>
<style scoped>
  #by-pry{
    border-radius: 0 0 150px 0;
  }
</style>