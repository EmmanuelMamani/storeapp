<template>
  <div>
    <button class="p-2 bg-blue-600 rounded-full bloque" @click="dialogCreate = true">
      <img src="../components/icons/add.svg" alt="Agregar" />
    </button>

    <el-dialog v-model="dialogCreate" width="500" @closed="dialogCreate = false">
      <span class="block mb-2 text-xl text-center">Crear Almacén</span>

      <Form @submit="crear" class="grid w-11/12 grid-cols-4 gap-2 mx-auto" :validation-schema="schema" v-slot="{ errors }">
        <label>Nombre:</label>
        <Field name="nombre" type="text" v-model="form.nombre" :class="{'border border-red-500':errors.nombre}" class="w-full col-span-3 p-1 rounded bg-slate-100" />
        <span class="col-span-3 col-start-2 text-red-500" v-if="errors.nombre">{{ errors.nombre }}</span>
        <label>Color:</label>
        <input type="color" v-model="form.color" class="block w-full h-8 col-span-3 rounded" />
        <span @click="dialogCreate = false" class="w-full col-start-3 p-2 text-center text-white bg-red-500 rounded cursor-pointer">Cancelar</span>
        <button type="submit" class="w-full p-2 text-white rounded bg-sky-500" >Crear</button>
      </Form>
    </el-dialog>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, defineEmits } from "vue";
import { ElNotification } from 'element-plus';
import { Form, Field } from 'vee-validate';
import * as yup from 'yup';

const schema = yup.object().shape({
  nombre: yup.string().required('El nombre del almacén es obligatorio'),
});
const emit = defineEmits(['actualizar']);
const dialogCreate = ref(false);
const form = ref({ nombre: '', color: '#000000' });

async function crear() {
  try {
    await schema.validate(form.value);
    await axios.post('http://localhost:8000/api/almacenes', {
      nombre: form.value.nombre,
      color: form.value.color
    });
    dialogCreate.value = false;
    form.value.nombre = '';
    form.value.color = '#000000';
    emit('actualizar');
    ElNotification({
      title: 'Éxito',
      message: 'El almacén fue creado exitosamente',
      type: 'success'
    })
  } catch (error) {
    console.error('Error al crear el almacén:', error);

    // Puedes manejar los errores de validación aquí si es necesario (por ejemplo, mostrarlos al usuario)
  }
}
</script>
