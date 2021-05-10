<script lang="ts">
  export let min: number = 0;
  export let max: number = 100;
  export let value: number = 0;
  export let step: number = 1;
  export let label = "";
</script>

<div>
  <label for="slider">{label}</label>
  <input id="slider" type="range" {min} {max} bind:value {step} />
</div>
