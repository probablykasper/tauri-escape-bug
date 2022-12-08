<script lang="ts">
  import { onDestroy } from "svelte";
  import { event } from "@tauri-apps/api";

  let keypresses: string[] = [];

  const unlistenFuture = event.listen("tauri://menu", ({ payload }) => {
    keypresses = [...keypresses, String(payload)];
  });
  onDestroy(async () => {
    const unlisten = await unlistenFuture;
    unlisten();
  });
</script>

<svelte:body
  on:keydown={(e) => {
    keypresses = [...keypresses, e.key];
  }}
/>

<h1>Keypresses</h1>
{#each keypresses as key}
  <span>{key} </span>
{/each}
