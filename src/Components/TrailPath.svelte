<script lang="ts">
  import {
    hoverPreview,
    openOrSwitch,
  } from "obsidian-community-lib/dist/utils";
  import type BCPlugin from "../main";
  import { dropDendron } from "../Utils/generalUtils";
  import { getAlt } from "../Utils/ObsidianUtils";

  export let sortedTrails: string[][];
  export let plugin: BCPlugin;

  const { settings, app } = plugin;
  const { view } = app.workspace.activeLeaf;
  let { showAll, noPathMessage, trailSeperator } = settings;

  $: trailsToShow = showAll ? sortedTrails : [sortedTrails[0]];
</script>

<span class="BC-trail-path-container">
  <div class="trails-div">
    {#each trailsToShow as trail}
      <div>
        {#if !trail.length}
          <span class="BC-empty-trail">{noPathMessage}</span>
        {:else}
          {#each trail as crumb, i}
            <span
              class="internal-link BC-Link"
              on:click={async (e) => await openOrSwitch(app, crumb, e)}
              on:mouseover={(e) => hoverPreview(e, view, crumb)}
            >
              {getAlt(crumb, plugin) ?? dropDendron(crumb, settings)}
            </span>
            {#if i < trail.length - 1}
              <span class="BC-trail-sep">{" " + trailSeperator + " "}</span>
            {/if}
          {/each}
        {/if}
      </div>
    {/each}
  </div>

  {#if sortedTrails.length > 1}
    <div>
      <button class="button-div" on:click={() => (showAll = !showAll)}>
        {showAll ? "Shortest" : "All"}
      </button>
    </div>
  {/if}
</span>

<style>
  span.BC-trail-path-container {
    display: flex;
    justify-content: space-between;
  }
</style>
