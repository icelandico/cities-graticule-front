<script lang="ts">
  import type { LatLng } from "leaflet";
  import Checkbox from "./components/Checkbox.svelte";
  import Map from "./components/Map.svelte";
  export type Option = 'meridian' | 'parallel';

  let chosenPoint: LatLng | null = null;
  let optionsChecked = {
    meridian: false,
    parallel: false,
  }

  function handleChangeOption(val: Option) {
    const currentValue = optionsChecked[val];
    optionsChecked[val] = !currentValue;
  }

  function handleSetPoint(point: LatLng) {
    chosenPoint = point;
  }
</script>

<div class="container">
  <h1>Cities on Graticule</h1>
  <Map handleSetPoint={handleSetPoint} />
  <div class="options-container">
    <Checkbox disabled={!chosenPoint} label="Parallel" value={"parallel"} bind:isChecked={optionsChecked.parallel} handleChange={handleChangeOption} />
    <Checkbox disabled={!chosenPoint} label="Meridian" value={"meridian"} bind:isChecked={optionsChecked.meridian} handleChange={handleChangeOption} />
  </div>
  {#if (!chosenPoint)}
    <h2 class="text-error">Select point first</h2>
  {/if}
</div>

<style>
  .container {
    align-items: center;
    max-width: 100%;
  }

  .options-container {
    width: 15%;
  }

  .text-error {
    color: rgb(204, 40, 19);
  }
</style>
