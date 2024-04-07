<template>
  <div class="p-3 text-center text-white cursor-pointer rounded-xl" :style="{ backgroundColor: almacen.color }">
    <RouterLink :to="{name:'almacen',params:{id:almacen.id}}">
      <span class="block text-xl font-medium">{{ almacen.nombre }}</span>
      <img src="./icons/almacen.svg" alt="" class="block mx-auto">
    </RouterLink>
    <div class="grid justify-center grid-cols-2">
      <button @click="dialogEdit=true" >Editar</button>
      <el-popconfirm confirm-button-text="Si" cancel-button-text="No" :icon="InfoFilled" icon-color="#626AEF" title="Seguro que quieres eliminar el almacen?" @confirm="eliminar">
        <template #reference>
          <button>Eliminar</button>
        </template>
      </el-popconfirm>
    </div>
  </div>
  <el-dialog v-model="dialogEdit"  width="500" @closed="cancelarEdit">
      <span class="block mb-2 text-xl text-center" >Actualizar Almacén</span>
      <div class="grid w-11/12 grid-cols-4 gap-2 mx-auto">
        <label>Nombre:</label><input type="text" v-model="almacen.nombre" class="w-full col-span-3 p-1 rounded bg-slate-300">
        <label>Color:</label><input type="color" v-model="almacen.color" class="block w-full h-8 col-span-3 rounded">
      </div>
    <template #footer>
      <div class="dialog-footer">
        <el-button @click="cancelarEdit">Cancel</el-button>
        <el-button type="primary" @click="actualizar">Actualizar</el-button>
      </div>
    </template>
  </el-dialog>
</template>
<script setup>
import axios from "axios";
import { defineProps, ref,defineEmits} from 'vue';
import { ElNotification } from 'element-plus'
import { InfoFilled } from '@element-plus/icons-vue'
import { RouterLink } from "vue-router";
const props = defineProps({
  almacen: Object,
});
const dialogEdit= ref(false)
const almacen = ref({...props.almacen});
const  emit = defineEmits(['actualizar']);
function eliminar(){
  axios.delete(`http://localhost:8000/api/almacenes/${almacen.value.id}`)
    .then(() => {
      ElNotification({
        title: 'Exito',
        message: 'El almacen fue eliminado exitosamente',
        type: 'success',
      })
      emit('actualizar');
    })
    .catch(error => {
      console.error('Error al eliminar el almacén:', error);
      // Aquí puedes manejar cualquier error que ocurra durante la eliminación
    });
}
async function actualizar() {
  try {
    await axios.put(`http://localhost:8000/api/almacenes/${almacen.value.id}`, {
      nombre: almacen.value.nombre,
      color: almacen.value.color
    });
    dialogEdit.value=false
    emit('actualizar');
    ElNotification({
        title: 'Exito',
        message: 'El almacen fue actualizado exitosamente',
        type: 'success',
      })
  } catch (error) {
    console.error('Error al actualizar el almacén:', error);
  }
}
function cancelarEdit(){
  dialogEdit.value=false
  almacen.value={...props.almacen}
}
</script>
