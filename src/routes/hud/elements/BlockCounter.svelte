<script lang="ts">
    import {listen} from "../../../integration/ws";
    import {fly} from "svelte/transition";
    import type {BlockCountChangeEvent} from "../../../integration/events";
    import {mapToColor} from "../../../util/color_utils";

    let count = $state<number | undefined>(undefined);

    listen("blockCountChange", (data: BlockCountChangeEvent) => {
        count = data.count;
    });
</script>

{#if count !== undefined}
    <div class="counter" style="color: {mapToColor(count)}" in:fly={{ y: -5, duration: 200 }}
         out:fly={{ y: -5, duration: 200 }}>
        <span class="text">Blocks:</span>
        {count}
    </div>
{/if}

<style lang="scss">
    @use "../../../colors.scss" as *;

    .counter {
        background-color: rgba($background-color, $opacity);
        border-radius: 12px;
        padding: 5px 8px;
        font-weight: 500;
        //border: $border-thing;
        box-shadow: $primary-shadow;
        text-shadow: $primary-shadow;

        .text {
            color: white;
        }
    }
</style>
