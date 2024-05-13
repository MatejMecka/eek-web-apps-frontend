<script lang="ts">
    import { faQuestion } from "@fortawesome/free-solid-svg-icons";
    import Fa from "svelte-fa";
    import PlushieImage from "./PlushieImage.svelte";

    export let selectedResponse;
    export let elemResponse;
    export let teddyBarcode;
    export let name;
    export let description
    export let imageId;

    const showTeddy = function(){
        console.log(elemResponse)
        selectedResponse = elemResponse;
        console.log(selectedResponse);
        teddy_info_modal.showModal()
    }

</script>

<div class="card lg:card-side bg-base-100 shadow-xl w-1/4 m-10">
    <PlushieImage imageId={imageId}></PlushieImage>
    <div class="card-body">
      <h2 class="card-title">{name}</h2>
      <p>{description}</p>
      <div class="card-actions justify-end">
        <button class="btn btn-primary" on:click={showTeddy}>View More</button>
      </div>
    </div>
</div>