<template>
  <div>
    <div id="map"></div>
    <button @click="getLocation">Obtener mi ubicación</button>
    <p v-if="userPosition">
      Latitud: {{ userPosition.lat }}, Longitud: {{ userPosition.lng }}
    </p>
  </div>
</template>

<script lang="ts" setup>
declare const google: any;
import { ref } from "vue";

// interface Position {
//   lat: number;
//   lng: number;
// }

const userPosition = ref<{ lat: number; lng: number } | null>(null);

function getMap(lat: number, lng: number) {
  const coord = { lat, lng };
  const map = new google.maps.Map(
    document.getElementById("map") as HTMLElement,
    {
      zoom: 10,
      center: coord,
    }
  );
  new google.maps.Marker({
    position: coord,
    map: map,
  });
}

function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      (position) => {
        const lat = position.coords.latitude;
        const lng = position.coords.longitude;
        userPosition.value = { lat, lng };
        getMap(lat, lng);
        console.log(lat, lng);
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

<style scoped>
#map {
  height: 200px;
  width: 100%;
}
</style>
