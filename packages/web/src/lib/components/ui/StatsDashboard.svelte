<script lang="ts">
import { getWorldStatistics } from "$lib/world/query";
import type { StatsDto } from "@formicarium/domain";

let {
  class: className = "",
  onclose,
}: {
  class?: string;
  onclose: () => void;
} = $props();

let worldStats = $state<StatsDto | undefined>(undefined);

$effect(() => {
  const interval = setInterval(async () => {
    worldStats = await getWorldStatistics();
  }, 1000);

  getWorldStatistics().then((stats) => {
    worldStats = stats;
  });

  return () => clearInterval(interval);
});
</script>

<div
  class={`relative bg-stone-800 text-white border-t border-4 border-stone-900 p-4 ${className}`}
>
  <button
    onclick={onclose}
    class="absolute top-2 right-2 text-white hover:text-gray-400 cursor-pointer"
    aria-label="close"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      class="h-6 w-6"
      fill="none"
      viewBox="0 0 24 24"
      stroke="currentColor"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        d="M6 18L18 6M6 6l12 12"
      />
    </svg>
  </button>
  <h2 class="text-xl font-bold mb-4">World statistics</h2>
  {#if worldStats}
    <h4 class="text-md font-bold mr-4">
      Alive ants: <span class="font-normal">{worldStats.antCount}</span>
    </h4>
    <h4 class="text-md font-bold mr-4">
      Food sources: <span class="font-normal">{worldStats.foodSourceCount}</span>
    </h4>
  {:else}
    <p>Statistics are unavailable, something must have gone wrong ...</p>
  {/if}
</div>
