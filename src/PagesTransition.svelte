<script>
  import { onMount } from 'svelte'
  import { fly } from 'svelte/transition'
  import { quintOut } from 'svelte/easing'

  let show = false

  onMount(() => show = true)
</script>

<style>
  main {
    position: fixed;
    overflow-y: scroll;
    overflow-x: hidden;
    width: 101%;
    height: 100%;
  }
</style>

<section>
  {#if show}
    <main in:fly="{{ y: 960, duration: 1000, opacity: 0, easing: quintOut }}">
      <slot></slot>
    </main>
  {/if}     
</section>