<script setup lang="ts">
import { ref } from "vue";
import MapG from "./components/MapsG.vue";
interface Position {
  lat: number;
  lng: number;
}

const userPosition = ref<Position | null>(null);
const buttonDisabled = ref(false)
const invisibleState = ref(false)

function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      (pos) => {
        const lat = pos.coords.latitude;
        const lng = pos.coords.longitude;
        userPosition.value = { lat, lng };
        console.log(lat, lng);
        buttonDisabled.value = true;
        invisibleState.value=true;
      },
      (error) => {
        console.error("Error al obtener la ubicación: ", error);
      }
    );
  } else {
    console.error("Geolocalización no es soportada por este navegador.");
  }
}
</script>

<template>
  <div>
    <button :disabled="buttonDisabled" @click="getLocation">Obtener mi ubicación</button>
    <p v-if="userPosition">
      Tu posicion:
      Latitud: {{ userPosition.lat }}, Longitud: {{ userPosition.lng }}
    </p>
    <!-- Mostrar mapa solo si hay coordenadas -->
    <mapG v-if="userPosition" :lat="userPosition.lat" :lng="userPosition.lng" />
  </div>
  <div>
    <input type="text" v-if="invisibleState"></input>
  </div>
  <div>
    <button v-if="invisibleState">Agregar nombre de parcela</button>
  </div>
</template>

<style scoped>

</style>
