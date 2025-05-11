<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Bucharest 3D Street Map</title>
  <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
  <script src="https://api.mapbox.com/mapbox-gl-js/v2.9.1/mapbox-gl.js"></script>
  <link href="https://api.mapbox.com/mapbox-gl-js/v2.9.1/mapbox-gl.css" rel="stylesheet" />
  <style>
    body { margin: 0; padding: 0; }
    #map { position: absolute; top: 0; bottom: 0; width: 100%; }
  </style>
</head>
<body>
<div id="map"></div>
<script>
  mapboxgl.accessToken = 'YOUR_MAPBOX_ACCESS_TOKEN';
  const map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/streets-v12',
    center: [26.1025, 44.4268], // Bucharest
    zoom: 14,
    pitch: 60,
    bearing: -10,
    antialias: true
  });

  map.on('load', () => {
    map.setTerrain({source: 'mapbox-dem', exaggeration: 1.5});
    map.addSource('mapbox-dem', {
      type: 'raster-dem',
      url: 'mapbox://mapbox.terrain-rgb',
      tileSize: 512,
      maxzoom: 14
    });

    map.addLayer({
      'id': '3d-buildings',
      'source': 'composite',
      'source-layer': 'building',
      'filter': ['==', 'extrude', 'true'],
      'type': 'fill-extrusion',
      'minzoom': 15,
      'paint': {
        'fill-extrusion-color': '#aaa',
        'fill-extrusion-height': ['get', 'height'],
        'fill-extrusion-base': ['get', 'min_height'],
        'fill-extrusion-opacity': 0.6
      }
    });
  });
</script>
</body>
</html>
