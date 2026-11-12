<template>
  <div class="map-wrap">
    <a href="https://www.maptiler.com" class="watermark"><img src="https://api.maptiler.com/resources/logo.svg"
        alt="MapTiler logo" /></a>
    <div class="map" ref="mapContainer"></div>
  </div>
</template>

<script>
import { Map as MapLibreMap, NavigationControl, Marker, Popup } from 'maplibre-gl';
import 'maplibre-gl/dist/maplibre-gl.css';
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

      map.value = markRaw(new MapLibreMap({
        container: mapContainer.value,
        style: `https://api.maptiler.com/maps/01a1d029-0f3b-7358-971e-387ed5eb053f/style.json?key=${apiKey}`,
        center: [initialState.lng, initialState.lat],
        zoom: initialState.zoom
      }));

      map.value.addControl(new NavigationControl(), 'top-right');

      suppliers.forEach(supplier => {
        if (!supplier.location) return

        const { lat, lng } = supplier.location

        const content = document.createElement('div')

        const name = document.createElement('strong')
        name.textContent = supplier.name
        content.append(name)

        if (supplier.website) {
          const link = document.createElement('a')
          link.href = supplier.website
          link.target = '_blank'
          link.rel = 'noopener'
          link.textContent = supplier.website
          content.append(document.createElement('br'), link)
        }

        const popup = new Popup({ offset: 25 }).setDOMContent(content)

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
  height: 100%;
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
