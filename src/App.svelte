<script lang="ts">
  import type { LatLng } from "leaflet";
  import type { Option } from './types'
  import Map from "./components/Map.svelte";

  let drawParallelChild: (val: number) => void;
  let drawMeridianChild: (val: number) => void;
  let removeMeridianChild: () => L.Polyline;
  let removeParallelChild: () => L.Polyline;

  let chosenPoint: LatLng | null = null;
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

  function handleSetPoint(point: LatLng) {
    chosenPoint = point;
  }
</script>

<div class="container">
  <h1>Cities on Graticule</h1>
  <h2 class="text-error">Use searchbar to find a point</h2>
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

  .text-error {
    color: rgb(204, 40, 19);
  }
</style>
