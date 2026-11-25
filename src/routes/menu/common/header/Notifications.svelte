<script lang="ts">
    import {fly} from "svelte/transition";
    import {notification, type TNotification} from "./notification_store";
    import {onMount, tick} from "svelte";

    let current: TNotification | null = null;
    let pending: TNotification | null = null;
    let visible = false;
    let leaving = false;
    let mounted = false;
    let dismissTimeout: ReturnType<typeof setTimeout> | undefined;

    function showPending() {
        if (!mounted) {
            return;
        }

        current = pending;
        pending = null;
        leaving = false;
        visible = current !== null;
    }

    async function hideCurrent() {
        clearTimeout(dismissTimeout);
        if (leaving || !current) {
            return;
        }

        leaving = true;
        await tick();
        if (mounted) {
            visible = false;
        }
    }

    function scheduleDismiss() {
        if (!mounted || leaving || !visible || !current) {
            return;
        }

        dismissTimeout = setTimeout(hideCurrent, (current.delay ?? 3) * 1000);
    }

    onMount(() => {
        mounted = true;
        const unsubscribe = notification.subscribe((value) => {
            if (!value) {
                return;
            }

            pending = {...value};
            notification.set(null);

            if (current) {
                hideCurrent();
            } else {
                showPending();
            }
        });

        return () => {
            mounted = false;
            unsubscribe();
            clearTimeout(dismissTimeout);
        };
    });
</script>

<div class="notifications">
    {#if visible && current}
        <div class="notification"
             transition:fly|global={{duration: 500, y: -100}}
             on:introend={scheduleDismiss}
             on:outroend={showPending}>
            <div class="icon" class:error={current.error}>
                <img src="img/hud/notification/icon-info.svg" alt="info">
            </div>
            <div class="title">{current.title}</div>
            <div class="message">{current.message}</div>
        </div>
    {/if}
</div>

<style lang="scss">
  @use "../../../../colors.scss" as *;


  .notifications {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    min-width: 350px;
    display: grid;
    grid-template-columns: 1fr;
    z-index: 999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999;
  }

  .notification {
    grid-row-start: 1;
    grid-column-start: 1;
    background-color: rgba($background-color, $opacity);
    border-radius: 12px;
    display: grid;
    grid-template-areas:
      "a b"
      "a c";
    grid-template-columns: max-content minmax(0, 1fr);
    overflow: hidden;
    padding-right: 7px;
    //border: $border-thing;
    box-shadow: $primary-shadow;
    text-shadow: $primary-shadow;

    .title, .message {
      overflow-wrap: anywhere;
    }

    .title {
      color: $text-color;
      font-weight: 600;
      font-size: 18px;
      grid-area: b;
      align-self: flex-end;
    }

    .message {
      color: $text-dimmed-color;
      font-weight: 400;
      grid-area: c;
    }

    .icon {
      grid-area: a;
      height: 55px;
      width: 55px;
      //background-color: rgba($accent-color, 0.5);
      display: flex;
      align-items: center;
      justify-content: center;

      &.error {
        background-color: var(--menu-header-notification-icon-error-background-color);
      }
    }
  }
</style>
