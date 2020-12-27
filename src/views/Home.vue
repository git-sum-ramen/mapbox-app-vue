<template>
  <div class="home">
    <div id="map"></div>
    <h1>{{ message }}</h1>
  </div>
</template>

<style>
	body { margin: 0; padding: 0; }
	#map { position: absolute; top: 0; bottom: 0; width: 800px; height: 500px; }
  #marker {
background-image: url('https://docs.mapbox.com/mapbox-gl-js/assets/washington-monument.jpg');
background-size: cover;
width: 50px;
height: 50px;
border-radius: 50%;
cursor: pointer;
}
 
.mapboxgl-popup {
max-width: 200px;
}
  
</style>

<script>
/* global mapboxgl */

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!", 
      places: [
        { lat: -25.363, lng: 131.044, description: "A place in Australia" },
        { lat: -33.8675, lng: 151.207, description: "The main city!" }
      ]     
    };
  },
  mounted: function() {
    var monument = [-77.0353, 38.8895];

    console.log(process.env.VUE_APP_MY_API_KEY);
    mapboxgl.accessToken = process.env.VUE_APP_MY_API_KEY;
    var map = new mapboxgl.Map({
      container: 'map', // container id
      // style: 'mapbox://styles/mapbox/outdoors-v11', // style URL
      style: 'mapbox://styles/mapbox/satellite-v9', // style URL
      center: monument, // starting position [lng, lat]
      zoom: 0 // starting zoom
    });

    this.places.forEach(function(place) {
      var popup = new mapboxgl.Popup({ offset: 25 }).setText(
        place.description
        );
        
        // create DOM element for the marker
        var el = document.createElement('div');
        el.id = 'marker';
        
        // create the marker
        new mapboxgl.Marker(el)
        .setLngLat([place.lng, place.lat])
        .setPopup(popup) // sets a popup on this marker
        .addTo(map);
      })


    
  },
  methods: {}
};
</script>
