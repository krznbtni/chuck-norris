<script lang="ts">
  export let isOpen = false;
  export const open = () => (isOpen = true);
  export const close = () => (isOpen = false);

  function handler(e: KeyboardEvent) {
    if (isOpen && e.key === 'Escape') close();
  }
</script>

<svelte:window on:keydown={handler} />

<slot name="trigger" {open}>
  <button on:click={open}>Open</button>
</slot>

{#if isOpen}
  <div class="modal-wrapper">
    <div class="modal-backdrop" on:click={close} />

    <div class="modal-content">
      <slot name="content" />

      <slot name="footer" {close}>
        <div>
          <button on:click={close}>close</button>
        </div>
      </slot>
    </div>
  </div>
{/if}

<style>
  .modal-wrapper {
    position: fixed;
    top: 0;
    right: 0;
    z-index: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
  }

  .modal-backdrop {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: black;
    opacity: 0.5;
  }

  .modal-content {
    background-color: white;
    width: 80%;
    border-radius: 10px;
    z-index: 1;
    border-color: grey;
    margin: 15% auto;
    padding: 20px;
  }

  @media (min-width: 1024px) {
    .modal-content {
      width: 50%;
    }
  }
</style>
