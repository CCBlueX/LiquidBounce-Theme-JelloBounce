<script lang="ts">
    import {createEventDispatcher} from "svelte";

    export let value: boolean;
    export let name: string;

    const dispatch = createEventDispatcher();
</script>

<label class="switch-container">
    <span class="switch">
        <input type="checkbox" bind:checked={value} on:change={() => dispatch("change")}/>
        <span class="slider"></span>
    </span>

    <span class="name">{name}</span>
</label>

<style lang="scss">
  @use "../../../../colors.scss" as *;


  .switch-container {
    display: flex;
    align-items: center;
  }

  .name {
    font-weight: 500;
    color: $text-color;
    font-size: 12px;
    margin-left: 5px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .slider {
    position: absolute;
    top: 2px;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba($text-color, 0.2);
    transition: ease 0.25s;
    height: 12px;
    width: 12px;
    border-radius: 50%;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }

  .switch {
    position: relative;
    width: 15px;
    height: 12px;

    input {
      display: none;
    }

    input:checked + .slider {
      background-color: $text-color;
      box-shadow: 0 0 8px rgba($text-color, 0.6);
    }
  }
</style>
