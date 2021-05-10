<script lang="ts">
  import { afterUpdate, onMount } from "svelte";
  import Slider from "./components/Slider.svelte";
  import { buildFlock, FollowCluster } from "./lib/Example";
  import type { Flock } from "./lib/Flock";
  let canvas: HTMLCanvasElement;
  let width: number;
  let ctx: CanvasRenderingContext2D;

  let flockConfig = {
    ctx,
    flockSize: 100,
    baseAffinity: -20,
    baseSpeed: 1,
    randomSpeedMultiplier: 5,
    speedFluctuationMultiplier: 0.05,
  };

  let configDetails = [
    {
      label: "Flock Size",
      name: "flockSize",
      default: 100,
      min: 2,
      max: 200,
      step: 1,
    },
    {
      label: "Base Affinity",
      name: "baseAffinity",
      default: -20,
      min: -50,
      max: 50,
      step: 1,
    },
    {
      label: "Base Speed",
      name: "baseSpeed",
      default: 1,
      min: 1,
      max: 5,
      step: 0.1,
    },
    {
      label: "Random Speed Multiplier",
      name: "randomSpeedMultiplier",
      default: 5,
      min: 0,
      max: 10,
      step: 0.1,
    },
    {
      label: "Speed Fluctuation Multiplier",
      name: "speedFluctuationMultiplier",
      default: 0.05,
      min: 0,
      max: 1,
      step: 0.01,
    },
  ];

  let flock: Flock<
    {
      speed: number;
      affinity: number;
    },
    {
      mouseX: 0;
      mouseY: 0;
    }
  >;
  const mousePos = {
    mouseX: 0,
    mouseY: 0,
  } as const;

  const draw = () => {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    flock.update(mousePos);
    flock.draw();
    window.requestAnimationFrame(draw);
  };

  onMount(() => {
    ctx = canvas.getContext("2d");
    flockConfig.ctx = ctx;
    flock = buildFlock(flockConfig);
    draw();
  });

  afterUpdate(() => {
    canvas.width = Math.min(width - 100, 500);
  });

  $: flock = buildFlock(flockConfig);
</script>

<svelte:window bind:innerWidth={width} />

<div class="container">
  <aside class="config">
    <h1>Config</h1>
    {#each configDetails as option}
      <Slider
        label={`${option.label}: ${flockConfig[option.name]}`}
        bind:value={flockConfig[option.name]}
        max={option.max}
        min={option.min}
        step={option.step}
      />
    {/each}
    <!-- <Slider
      label={`Flock Size: ${flockConfig.flockSize}`}
      bind:value={flockConfig.flockSize}
      min={2}
      max={200}
    /> -->
  </aside>

  <main>
    <h1>Welcome to BODA</h1>
    <canvas bind:this={canvas} id="canvas" height="500" />
  </main>

  <aside class="about">
    <h1>About</h1>
  </aside>
</div>

<style>
  .container {
    display: grid;
    grid-template-columns: 1fr 600px 1fr;
    grid-template-areas: "config main about";
  }

  aside {
    display: flex;
    flex-direction: column;
    text-align: center;
  }

  .config {
    grid-area: config;
  }
  .about {
    grid-area: about;
  }
  main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    padding: 1em;
    /* max-width: 240px; */
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  canvas {
    background-color: rgb(231, 225, 225);
  }

  @media (max-width: 1060px) {
    .container {
      display: grid;
      grid-template-columns: 100%;
      grid-template-rows: 800px 1fr 1fr;
      grid-template-areas: "main" "config" "about";
    }
  }

  @media (max-width: 1060px) {
    .container {
      display: grid;
      grid-template-columns: 100%;
      grid-template-rows: 800px 1fr 1fr;
      grid-template-areas: "main" "config" "about";
    }
  }
</style>
