<script lang="ts">
import PixelWorldCanvas from "$lib/components/PixelWorldCanvas.svelte";
import AntStatsPanel from "$lib/components/ui/AntStatsPanel.svelte";
import DropdownMenu from "$lib/components/ui/DropdownMenu.svelte";
import Navbar from "$lib/components/ui/Navbar.svelte";
import SimulationControls from "$lib/components/ui/SimulationControls.svelte";
import { uiState } from "$lib/state/ui.svelte";
import { initialiseWorldAssets } from "$lib/world/assets";
import { onMount } from "svelte";

let isLoading = $state(true);

onMount(async () => {
  await initialiseWorldAssets();

  // Show connecting screen for fun
  setTimeout(() => {
    isLoading = false;
  }, 1000);
});
</script>

<div class="relative w-full h-full bg-amber-100">
  {#if isLoading}
    <div class="flex items-center justify-center h-full">
      <div class="text-center">
        <h2 class="text-2xl font-semibold text-stone-900 mb-2">
          Connecting<span class="animated-ellipsis"></span>
        </h2>
        <p class="text-stone-500">
          Do simulated ants dream of electric sheep?
        </p>
      </div>
    </div>
  {:else}
    <PixelWorldCanvas />
    <AntStatsPanel />
    {#if uiState.showSimulationControls}
      <SimulationControls />
    {/if}
  {/if}
</div>

<style>
  .animated-ellipsis::after {
    content: ".";
    animation: ellipsis 1.5s infinite;
  }

  @keyframes ellipsis {
    0% {
      content: ".";
    }
    33% {
      content: "..";
    }
    66% {
      content: "...";
    }
  }
</style>
