<template>
  <div id="map"></div>
</template>

<script lang="ts" setup>
declare const google: any;
import { onMounted, defineProps } from "vue";

const props = defineProps<{
  lat: number;
  lng: number;
}>();

function loadScript(src: string, callback: () => void) {
  const script = document.createElement("script");
  script.src = src;
  script.async = true;
  script.defer = true;
  script.onload = callback;
  document.head.appendChild(script);
}

function iniciarMap() {
  const coord = { lat: props.lat, lng: props.lng }; //recbir ubicacion actual
  const map = new google.maps.Map(
    document.getElementById("map") as HTMLElement,
    { zoom: 10, center: coord }
  );
  new google.maps.Marker({ position: coord, map: map });
}

onMounted(() => {
  loadScript(
    "https://maps.googleapis.com/maps/api/js?key=AIzaSyBDaeWicvigtP9xPv919E-RNoxfvC-Hqik&callback=iniciarMap",
    iniciarMap
  );
});
</script>

<style scoped>
#map {
  height: 500px;
  width: 100%;
  margin-left: 20px;
}
</style>
