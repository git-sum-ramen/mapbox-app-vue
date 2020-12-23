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
/* global mapboxgl */

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",      
    };
  },
  mounted: function() {
    console.log(process.env.VUE_APP_MY_API_KEY);
    mapboxgl.accessToken = process.env.VUE_APP_MY_API_KEY;
    var map = new mapboxgl.Map({
      container: 'map', // container id
      // style: 'mapbox://styles/mapbox/outdoors-v11', // style URL
      style: 'mapbox://styles/mapbox/satellite-v9', // style URL
      center: [-86.7816, 36.1627], // starting position [lng, lat]
      zoom: 0 // starting zoom
    });

    map.on('load', function () {
    // We use D3 to fetch the JSON here so that we can parse and use it separately
    // from GL JS's use in the added source. You can use any request method (library
    // or otherwise) that you want.
    d3.json(
    'https://docs.mapbox.com/mapbox-gl-js/assets/hike.geojson',
    function (err, data) {
    if (err) throw err;
    
    // save full coordinate list for later
    var coordinates = [
      // nashville
      [
          -86.7816, 
          36.1627
      ],
      // buffalo
      [
        -78.8784,
          42.8864
      ],
      [
        -122.4194,
          37.7749
      ],
      [
        -122.4194,
          37.7749
      ],
      [
        -87.6359,
        41.8789
      ]
    ];
    
    // start by showing just the first coordinate
    data.features[0].geometry.coordinates = [coordinates[0]];
    
    // add it to the map
    map.addSource('trace', { type: 'geojson', data: data });
    map.addLayer({
    'id': 'trace',
    'type': 'line',
    'source': 'trace',
    'paint': {
    'line-color': 'yellow',
    'line-opacity': 0.75,
    'line-width': 8
    }
    });
    
    // setup the viewport
    map.jumpTo({ 'center': coordinates[0], 'zoom': 2 });
    map.setPitch(30);
    
    // on a regular basis, add more coordinates from the saved list and update the map
    var i = 0;
    var timer = window.setInterval(function () {
        if (i < coordinates.length) {
          data.features[0].geometry.coordinates.push(
            coordinates[i]
          );
          map.getSource('trace').setData(data);
          map.panTo(coordinates[i]);
          i++;
        } else {
          window.clearInterval(timer);
        }
      }, 1000);
      }
      );
    });

  },
  methods: {}
};
</script>
