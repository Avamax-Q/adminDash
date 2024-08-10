<script lang="ts">
  import Login from "./lib/Login.svelte";
  //import Messages from "./lib/Messages.svelte";
  import { currentUser } from "./lib/pocketbase";
  import Dashboard from "./lib/Dashboard.svelte";


</script>

<Login />
{#if $currentUser}
    <Dashboard />
{/if}