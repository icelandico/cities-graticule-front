<script>
  import * as L from 'leaflet';
  import {Geocoder, geocoders} from 'leaflet-control-geocoder';
  import { onMount } from 'svelte';

  let leafletMap;

  onMount(() => {
    leafletMap = L.map('map').setView([54.364917, 18.422872], 3);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      minZoom: 0,
      maxZoom: 20,
      maxNativeZoom: 19,
      attribution: '© OpenStreetMap contributors',
    }).addTo(leafletMap);

    new Geocoder({
    geocoder: new geocoders.Nominatim(),
    position: 'topright',
  }).addTo(leafletMap);
  });

  function handleSearchCity(e) {
    e.preventDefault();
    console.log('Submit')
  }
</script>

<div class="container">
  <h1>Cities on Graticule</h1>
  <div class="map-container">
    <div id="map"></div>
  </div>

</div>

<style>
  .container {
    align-items: center;
    max-width: 100%;
  }
  .map-container {
    border-radius: 8px;
    height: 500px;
    margin-bottom: 2rem;
  }

  #map {
    height: 500px;
    border-radius: 8px;
    margin-bottom: 2rem;
  }
</style>
