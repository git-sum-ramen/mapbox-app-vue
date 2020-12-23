<template>
  <div class="home">
    <div id="map"></div>
    <h1>{{ message }}</h1>
  </div>
</template>

<style>
	body { margin: 0; padding: 0; }
	#map { position: absolute; top: 0; bottom: 0; width: 800px; height: 500px; }
</style>

<script>
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",      
    };
  },
  mounted: function() {
    var map = new mapboxgl.Map({
    container: 'map', // container id
    style: 'mapbox://styles/mapbox/outdoors-v11', // style URL
    center: [-86.7816, 36.1627], // starting position [lng, lat]
    zoom: 10 // starting zoom
    });
  },
  methods: {}
};
</script>
