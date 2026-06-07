---
title: "Map"
---

<div class="map-page-header">
  <div>
    <h1>Browse Geographically</h1>
    <p>Click any marker to read about a historic place in Marion and Grant County.</p>
  </div>
</div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css">
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css">
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>

<div id="full-map" style="width:100%;height:70vh;min-height:500px;border:1px solid #d4cfc4;"></div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  var map = L.map('full-map').setView([40.5584, -85.6591], 13);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors',
    maxZoom: 19
  }).addTo(map);
  fetch('/data/map_markers.json')
    .then(function(r) { return r.json(); })
    .then(function(data) {
      data.forEach(function(m) {
        L.marker([m.lat, m.lng]).addTo(map)
          .bindPopup('<strong><a href="' + m.url + '">' + m.title + '</a></strong>');
      });
    })
    .catch(function(err) {
      document.getElementById('full-map').innerHTML = '<p style="padding:2rem;">Error loading map data: ' + err.message + '</p>';
    });
});
</script>
