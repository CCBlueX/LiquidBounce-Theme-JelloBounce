<script lang="ts">
    export let vertical = false;
</script>

<div class="button-container" class:vertical>
  <slot />
</div>

<style lang="scss">
  @use "../../../../colors.scss" as *;

  .button-container {
    background: none;
    display: flex;
    column-gap: 15px;
    width: max-content;
    padding-top: 25px;

    &.vertical {
      flex-direction: column;
      row-gap: 15px;
      padding-top: 0;
    }
  }
</style>
