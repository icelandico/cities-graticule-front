<script lang="ts">
  import * as L from 'leaflet';
  import { Geocoder, geocoders } from 'leaflet-control-geocoder';
  import { onMount } from 'svelte';
  import type { UserPoint } from '../types';
  export let handleSetPoint: (point: UserPoint) => void;

  let map: L.Map;
  let parallelLine: L.Polyline;
  let meridianLine: L.Polyline;

  onMount(() => {
      map = L.map('map').setView([54.364917, 18.422872], 2);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        minZoom: 0,
        maxZoom: 15,
        maxNativeZoom: 15,
        attribution: '© OpenStreetMap contributors',
      }).addTo(map);

      new Geocoder({
        geocoder: new geocoders.Nominatim(),
        position: 'topright',
        collapsed: false
      }).on('markgeocode', function (e) {
        const { name, center, bbox } = e.geocode;
        handleSetPoint({
          name,
          coordinates: center
        });
        drawParallel(center.lat);
        drawMeridian(center.lng);
        map.setZoom(2)
      }).addTo(map);
  });

  export const drawParallel = (latitude: number) => {
    parallelLine = L.polyline([
        [latitude, -180],
        [latitude, 180]
      ], { color: '#24b529' }).addTo(map)
    }

  export const drawMeridian = (longitude: number) => {
    meridianLine = L.polyline([
        [90, longitude],
        [0, longitude],
        [-90, longitude]
      ], { color: '#54544d' }).addTo(map)
  }

  export const removeParallel = () => parallelLine.remove();
  export const removeMeridian = () => meridianLine.remove();
   
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
