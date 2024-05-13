<script lang="ts">
    import { faBarcode } from "@fortawesome/free-solid-svg-icons";
    import BarcodeModal from "./modals/BarcodeModal.svelte";
    import Alert from "./Alert.svelte";
    import Fa from "svelte-fa";

    let value = undefined;
    export let selectedResponse;
    let error;

    $: {
        if(value != undefined){
            console.log(value);
            const options = {method: 'GET'};

            fetch(`${import.meta.env.VITE_API_URL}teddybear/${value}`, options)
            .then(response => {
                if(!response.ok){
                    error = true;
                }
                return response.json()
            })
            .then(response => {
                console.log(response)
                if(response['id'] == undefined) {
                    return;
                }
                console.log(response)
                selectedResponse = response
                teddy_info_modal.showModal()
            
            })
            .catch(err => {
                error = true
                console.error(err)
            });
        }
    }
</script>

<div class="search flex items-center justify-center flex-col">
    <h1 class="m-5">Search a Plushie</h1>
    <Alert alert={error} alert_message={"There was an error accessing the database"} error={true} tagValue={"error"}></Alert>
    <br>
    <div class="flex flex-row">
        <label class="input input-bordered flex items-center gap-2">
            <input type="text" class="grow" placeholder="Search" bind:value={value} />
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="w-4 h-4 opacity-70"><path fill-rule="evenodd" d="M9.965 11.026a5 5 0 1 1 1.06-1.06l2.755 2.754a.75.75 0 1 1-1.06 1.06l-2.755-2.754ZM10.5 7a3.5 3.5 0 1 1-7 0 3.5 3.5 0 0 1 7 0Z" clip-rule="evenodd" /></svg>
        </label>
        {#if "BarcodeDetector" in globalThis}
            <button onclick="video_modal.showModal()" class="mx-4 btn">
                <Fa icon={faBarcode} />
                Scan Barcode
            </button>
            <BarcodeModal bind:barcode_val={value}></BarcodeModal>
        {/if}
    </div>
</div>



<style>
    .search {
        min-height: 100vh;
    }
</style>