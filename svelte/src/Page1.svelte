<script lang="ts">
  import { onMount } from "svelte";
  import "smelte/src/tailwind.css";
  import { TextField } from "svelte-materialify";
  export let params;
  let name = params.name;
  onMount(async () => {});
  function handleChange(e: any) {
    name = e.target.value;
  }
</script>

<main>
  <a href="#/page2/{Math.random()}">Page 2</a>
</main>
<TextField value={name} on:change={handleChange}>Title</TextField>

<style>
</style>
