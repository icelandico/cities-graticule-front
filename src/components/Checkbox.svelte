<script lang="ts" generics="T">
  import Option from './../App.svelte';
  export let value: T;
  export let isChecked: boolean;
  export let label: string;
  export let disabled = false;
  export let handleChange: (val: T) => void;
</script>

<label class="option-container">
  {label}
  <input disabled={disabled} type="checkbox" bind:checked={isChecked} on:change={() => handleChange(value)}>
  <span class="checkmark"></span>
</label>

<style>

.option-container {
    display: block;
    position: relative;
    margin-bottom: 12px;
    cursor: pointer;
    font-size: 18px;
    user-select: none;
  }

  .option-container input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }

  .checkmark {
    position: absolute;
    top: 0;
    left: 0;
    height: 25px;
    width: 25px;
    background-color: #eee;
  }

  .option-container:hover input ~ .checkmark {
    background-color: #ccc;
  }

  .option-container input:checked ~ .checkmark {
    background-color: #2196F3;
  }

  .checkmark:after {
    content: "";
    position: absolute;
    display: none;
  }

  .option-container input:checked ~ .checkmark:after {
    display: block;
  }

  .option-container .checkmark:after {
    left: 9px;
    top: 5px;
    width: 5px;
    height: 10px;
    border: solid white;
    border-width: 0 3px 3px 0;
    transform: rotate(45deg);
  }
</style>
