<script lang="ts">
    import MainButton from "../title/buttons/MainButton.svelte";
    import {openScreen, toggleBasicMode} from "../../../integration/rest";
</script>

<!-- Clicks also reach the vanilla screen below, so this stays in its empty left strip. -->
<div class="basic-menu">
    <div class="buttons">
        <MainButton title="Full Mode" icon="liquidbounce" on:click={toggleBasicMode} index={0}/>
        <MainButton title="ClickGUI" icon="clickgui" on:click={() => openScreen("clickgui")} index={1}/>
        <MainButton title="Accounts" icon="user" on:click={() => openScreen("altmanager")} index={2}/>
        <MainButton title="Proxies" icon="proxymanager" on:click={() => openScreen("proxymanager")} index={3}/>
    </div>
</div>

<style lang="scss">
  .basic-menu {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    display: flex;
    align-items: center;
  }

  .buttons {
    display: flex;
    flex-direction: column;
    row-gap: 25px;
    margin-left: 50px;
  }

  @media screen and (max-width: 1366px) {
    .buttons {
      zoom: 0.8;
    }
  }

  @media screen and (max-width: 1200px) {
    .buttons {
      zoom: 0.5;
    }
  }

  @media screen and (max-height: 1100px) {
    .buttons {
      zoom: 0.8;
    }
  }

  @media screen and (max-height: 700px) {
    .buttons {
      zoom: 0.5;
    }
  }

  @media screen and (max-height: 540px) {
    .buttons {
      zoom: 0.4;
    }
  }
</style>
