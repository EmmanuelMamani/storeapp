<template>
    <div class="text-xl text-center text-blue-600">Almacen: {{ almacen?almacen.nombre:'Cargando...' }}</div>
</template>

<script setup>
    import { ref, onMounted } from 'vue';
    import axios from 'axios';
    import { useRoute } from 'vue-router';

    const almacen = ref(null);
    const route = useRoute();

    const fetchAlmacen = async () => {
        try {
            const response = await axios.get(`http://localhost:8000/api/almacenes/${route.params.id}`);
            almacen.value = response.data;
        } catch (error) {
            console.error('Error al recuperar almacenes:', error);
        }
    };

    onMounted(fetchAlmacen);
</script>
