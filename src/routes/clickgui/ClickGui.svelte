<script lang="ts">
    import type {GroupedModules, Module} from "../../integration/types";
    import Panel from "./Panel.svelte";
    import Search from "./Search.svelte";
    import Description from "./Description.svelte";
    import {fade} from "svelte/transition";
    import {onMount} from "svelte";
    import {getModules} from "../../integration/rest";
    import {groupByCategory} from "../../integration/util";
    import {gridSize, showGrid} from "./clickgui_store";
    import ScaledClickGuiContent from "./ScaledClickGuiContent.svelte";

    let categories = $state<GroupedModules>({});
    let modules = $state<Module[]>([]);

    onMount(async () => {
        modules = await getModules();
        categories = groupByCategory(modules);
    });
</script>

<ScaledClickGuiContent>
    <div
            class="clickgui"
            class:grid={$showGrid}
            style="background-size: {$gridSize}px {$gridSize}px;"
            transition:fade|global={{duration: 200}}
    >
        <Description/>
        <Search modules={structuredClone($state.snapshot(modules))}/>

        {#each Object.entries(categories) as [category, modules], panelIndex (category)}
            <Panel {category} {modules} {panelIndex}/>
        {/each}
    </div>
</ScaledClickGuiContent>

<style lang="scss">
  @use "../../colors.scss" as *;

  $GRID_SIZE: 10px;

  .clickgui {
    overflow: hidden;
    position: absolute;
    will-change: opacity;
    inset: 0;
    z-index: 1;

    &.grid {
      z-index: 999999999;
      background-image: linear-gradient(
          to right,
          $clickgui-grid-color 1px,
          transparent 1px
        ),
        linear-gradient(to bottom, $clickgui-grid-color 1px, transparent 1px);
    }
  }
</style>
