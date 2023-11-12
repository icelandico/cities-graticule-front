<script lang="ts" context="module">
  import Checkbox from "./components/Checkbox.svelte";
  import Map from "./components/Map.svelte";
  export type Option = 'meridian' | 'parallel';

  let chosenCity = {};
  let optionsChecked = {
    meridian: false,
    parallel: false,
  }

  function handleChangeOption(val: Option) {
    const currentValue = optionsChecked[val];
    optionsChecked[val] = !currentValue;
  }

  function handleSetPoint(point: any) {
    chosenCity = point;
  }
</script>

<div class="container">
  <h1>Cities on Graticule</h1>
  <Map handleSetPoint={handleSetPoint} />
  <div class="options-container">
    <Checkbox label="Parallel" value={"parallel"} bind:isChecked={optionsChecked.parallel} handleChange={handleChangeOption} />
    <Checkbox label="Meridian" value={"meridian"} bind:isChecked={optionsChecked.meridian} handleChange={handleChangeOption} />
  </div>
</div>

<style>
  .container {
    align-items: center;
    max-width: 100%;
  }

  .options-container {
    width: 15%;
  }
</style>
