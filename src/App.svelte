<script lang="ts">
  import { afterUpdate, onMount } from "svelte";
  import Slider from "./components/Slider.svelte";
  import { FollowCluster } from "./lib/Example";
  import type { Flock } from "./lib/Flock";
  let canvas: HTMLCanvasElement;
  let width: number;
  let ctx: CanvasRenderingContext2D;
  let flockSize: number = 10;
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
    flock = FollowCluster(ctx, flockSize);
    draw();
  });

  afterUpdate(() => {
    canvas.width = Math.min(width - 100, 500);
  });

  $: flock = FollowCluster(ctx, flockSize);
</script>

<svelte:window bind:innerWidth={width} />

<div class="container">
  <aside class="config">
    <h1>Config</h1>
    <Slider label="Flock Size" bind:value={flockSize} />
    {flockSize}
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
