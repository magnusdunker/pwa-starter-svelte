<script>
  import Page1 from "./Page1.svelte";
  import Page2 from "./Page2.svelte";
  import Router from "svelte-spa-router";
  import { MaterialApp } from "svelte-materialify";

  const routes = {
    "/page1/:name": Page1,
    "/page2/:name": Page2,
    "*": Page1,
  };
  let theme = "light";
</script>

<style>

</style>

<MaterialApp {theme}>
  <Router {routes} />
</MaterialApp>
