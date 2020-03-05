<script>
  import Nav from "../components/Nav.svelte";

  export let segment;
</script>

<style>
  main {
    position: relative;
    background-color: white;
    padding: 2em;
    margin: 120px 40px 0 240px;
    box-sizing: border-box;
    border-radius: 4px;
  }
</style>

<Nav {segment} />

<main>
  <slot />
</main>
