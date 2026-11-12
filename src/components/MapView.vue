<template>
  <div class="map-wrap">
    <a href="https://www.maptiler.com" class="watermark"><img src="https://api.maptiler.com/resources/logo.svg"
        alt="MapTiler logo" /></a>
    <div class="map" ref="mapContainer"></div>
  </div>
</template>

<script>
import * as maplibregl from 'maplibre-gl';
import { NavigationControl, Marker, Popup } from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css'
import { shallowRef, onMounted, onUnmounted, markRaw } from 'vue';
import suppliers from '../data/suppliers.json';

export default {
  name: "MapView",
  setup() {
    const mapContainer = shallowRef(null);
    const map = shallowRef(null);

    onMounted(() => {
      const apiKey = import.meta.env.VITE_MAPTILER_KEY;

      const initialState = { lng: 10, lat: 50, zoom: 3.5 };

      map.value = markRaw(new maplibregl.Map({
        container: mapContainer.value,
        style: `https://api.maptiler.com/maps/streets-v4/style.json?key=${apiKey}`,
        center: [initialState.lng, initialState.lat],
        zoom: initialState.zoom
      }));

      map.value.addControl(new NavigationControl(), 'top-right');

      suppliers.forEach(supplier => {
        if (!supplier.location) return

        const { lat, lng } = supplier.location

        const popup = new Popup({ offset: 25 }).setHTML(
          `<strong>${supplier.name}</strong><br>
           <a href="${supplier.website}" target="_blank" rel="noopener">${supplier.website}</a>`
        )

        new Marker({ color: "#FF0000" })
          .setLngLat([lng, lat])
          .setPopup(popup)
          .addTo(map.value)
      })

    })

    onUnmounted(() => {
      map.value?.remove();
    })

    return {
      map, mapContainer
    };
  }
};
</script>

<style scoped>
.map-wrap {
  position: relative;
  width: 100%;
  height: calc(100vh - 77px);
}

.map {
  position: absolute;
  width: 100%;
  height: 100%;
}

.watermark {
  position: absolute;
  left: 10px;
  bottom: 10px;
  z-index: 999;
}
</style>
