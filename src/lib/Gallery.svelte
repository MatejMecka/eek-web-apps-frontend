<script lang="ts">
    import { faTriangleExclamation } from "@fortawesome/free-solid-svg-icons";
    import PlushieCard from "./PlushieCard.svelte";
    import Fa from "svelte-fa";

    export let selectedResponse;

    async function fetchData() {
        const res = await fetch(import.meta.env.VITE_API_URL + 'teddybear/all');
        const data = await res.json();

        if (res.ok) {
        return data;
        } else {
        throw new Error(data);
        }
  }

</script>
<h1 class="m-5">Gallery</h1>
{#await fetchData()}
    <span class="loading loading-spinner loading-lg text-primary"></span>
{:then elems}
<div class="flex align-items flex-row flex-wrap">
    {#each elems as elem}
        <PlushieCard bind:selectedResponse elemResponse={elem} teddyBarcode={elem["teddyBarcode"]} name={elem["name"]} description={elem["description"]} imageId={elem["imageId"]} ></PlushieCard>
    {/each}
</div>
{:catch}
    <div class="flex justify-center align-items flex-col m-5">
        <Fa icon={faTriangleExclamation} scale={3}/>
        <h2 class="text-error text-center text-5xl m-5">Error!</h2>
        <h4 class="text-center">There was an error fetching the gallery!</h4>
    </div>
{/await}