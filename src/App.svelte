<script lang="ts">
  import type { LatLng } from "leaflet";
  import type { Option, UserPoint } from './types'
  import Map from "./components/Map.svelte";
  import { convertDMS } from "./utils/convertToDMS";
  import 'leaflet/dist/leaflet.css';

  let drawParallelChild: (val: number) => void;
  let drawMeridianChild: (val: number) => void;
  let removeMeridianChild: () => L.Polyline;
  let removeParallelChild: () => L.Polyline;

  let chosenPoint: UserPoint | null = null;
  let optionsChecked = {
    meridian: false,
    parallel: false,
  }

  function handleChangeOption(val: Option) {
    const currentValue = optionsChecked[val];
    optionsChecked[val] = !currentValue;
    if (val === 'meridian') {
      currentValue && removeMeridianChild();
      !currentValue && drawMeridianChild(chosenPoint?.lng)
    }

    if (val === 'parallel') {
      currentValue && removeParallelChild();
      !currentValue && drawParallelChild(chosenPoint?.lat)
    }
  }

  function handleSetPoint(point: UserPoint) {
    if (chosenPoint) {
      removeMeridianChild();
      removeParallelChild();
    }
    chosenPoint = point;
  }
</script>

<div class="container">
  <h1>Points on Graticule</h1>
  {#if chosenPoint}
    <div class="place__data">
      <h2>Place: {chosenPoint.name}</h2>
      <h2>Coordinates (DD): Lat: {chosenPoint?.coordinates.lat.toFixed(3)}, Lng: {chosenPoint?.coordinates.lng.toFixed(3)}</h2>
      <h2>Coordinates (DMS): {convertDMS(chosenPoint.coordinates.lat, chosenPoint.coordinates.lng)}</h2>
    </div>
  {/if}
  
  <Map
    bind:drawMeridian={drawMeridianChild}
    bind:drawParallel={drawParallelChild}
    bind:removeParallel={removeParallelChild}
    bind:removeMeridian={removeMeridianChild}
    handleSetPoint={handleSetPoint}
  />
</div>

<style>
  .container {
    align-items: center;
    max-width: 100%;
  }

  .place__data {
    text-align: left;
  }
</style>
