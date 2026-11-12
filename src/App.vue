<template>
  <Navbar />
  <MapView />
</template>

<script>
import Navbar from './components/Navbar.vue';
import MapView from './components/MapView.vue';

export default {
  name: "App",
  components: {
    Navbar,
    MapView
  },
};
</script>

<style>
body {
  margin: 0;
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  display: grid;
  grid-template-rows: auto 1fr;
  height: 100dvh;
}

code {
  font-family:
    source-code-pro, Menlo, Monaco, Consolas, "Courier New", monospace;
}

.app {
  text-align: center;
}
</style>
