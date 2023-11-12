<script lang="ts">
  import * as L from 'leaflet';
  import { Geocoder, geocoders } from 'leaflet-control-geocoder';
  import { onMount } from 'svelte';
  export let handleSetPoint: (point: L.LatLng) => void;

  onMount(() => {
    const leafletMap = L.map('map').setView([54.364917, 18.422872], 3);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      minZoom: 0,
      maxZoom: 20,
      maxNativeZoom: 19,
      attribution: '© OpenStreetMap contributors',
    }).addTo(leafletMap);

    new Geocoder({
      geocoder: new geocoders.Nominatim(),
      position: 'topright',
    }).on('markgeocode', function (e) {
      const { name, center } = e.geocode;
      handleSetPoint(center);
    }).addTo(leafletMap);
  });
</script>

<div class="map-container">
  <div id="map"></div>
</div>

<style>
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
