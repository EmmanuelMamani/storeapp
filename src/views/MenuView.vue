<template>
  <div class="relative w-5/6 mx-auto mt-3" id="contenedor">
    <span class="block text-xl text-center text-slate-500 ">Mis Almacenes</span>
    <div class="mb-20" v-if="almacenes.length === 0">
      <img src="../components/icons/sin_almacen.svg" class="block mx-auto">
      <span class="block text-center text-slate-500 text-md">Sin Almacenes</span>
    </div>
    <div class="grid justify-center grid-cols-2 gap-2 mt-5 md:grid-cols-4" :key="almacenes.length">
         <AlmacenComp v-for="(almacen, index) in almacenes" :almacen="almacen" :key="index" @actualizar="actualizarAlmacenes"></AlmacenComp>
    </div>
    <div class="absolute bottom-1 right-1">
      <AlmacenCreate @actualizar="actualizarAlmacenes"></AlmacenCreate>
      <button class="block p-2 my-3 bg-blue-600 rounded-full">
        <img src="../components/icons/file.svg" alt="exportar">
      </button>
    </div>
  </div>
  
</template>
<style scoped>
  #contenedor {
    min-height: 85vh;
  }
</style>
<script setup>
import AlmacenComp from "@/components/AlmacenComp.vue";
import AlmacenCreate from "@/components/AlmacenCreate.vue";
import { ref, onMounted} from "vue";
import axios from "axios";
const almacenes = ref([]);
const fetchAlmacenes = async () => {
    try {
      const response = await axios.get('http://localhost:8000/api/almacenes');
      almacenes.value = response.data;
    } catch (error) {
      console.error('Error al recuperar almacenes:', error);
    }
};
const actualizarAlmacenes = () => {
  fetchAlmacenes(); // Volver a cargar la lista de almacenes
};
onMounted(fetchAlmacenes);
</script>
