<script lang="ts">
  import GlobeComponent from './components/GlobeComponent.svelte';
</script>

<main class="w-screen h-screen">
  <GlobeComponent />
</main>

<style>
  :global(html, body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
  }
</style>