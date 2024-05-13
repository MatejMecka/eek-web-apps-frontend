<script lang="ts">
  import { MapLibre, DefaultMarker } from 'svelte-maplibre';
  import PlushieImage from "../PlushieImage.svelte";
  import DeleteButton from '../forms/DeleteButton.svelte';
  import Alert from '../Alert.svelte';

  let files;
  export let teddyData = undefined;

  const SUCCESS_MESSAGE = "Teddy Bear Updated Successfully!"
  const SUCCESS_UPDATE_MESSAGE_IMAGE = "Image successfully Updated."

  let alert:Boolean = false;
  let alert_message: String;
  let tagValue: String
  let error: Boolean;
  let changed = false;


  const updateData = function() {
    console.log(changed)
    if(teddyData == undefined || !changed){
      return
    }

    const options = {
      method: 'PATCH',
      headers: {'Content-Type': 'application/json'},
      body: JSON.stringify(teddyData)
    };

    fetch(`${import.meta.env.VITE_API_URL}teddybear/${teddyData["teddyBarcode"]}`, options)
      .then(response => {
        if(!response.ok){
          alert_message = "There was an error updadting data!"
          alert = true
          tagValue = 'error'
          error = true
        }
        return response.text()
      })
      .then(response => {
        console.log(response)
        if (response == SUCCESS_MESSAGE){
          alert_message = SUCCESS_MESSAGE
          alert = true
          tagValue = 'success'
          error = false
        } 
      })
      .catch(err => console.error(err));
      }

  const updateImage = function(){
    const form = new FormData();
    form.append("image", files[0]);

    if (form.has('image') == false) {
      return
    }

      const options = {
        method: 'PATCH',
        body: form
      };

      fetch(`${import.meta.env.VITE_API_URL}images/${teddyData['imageId']}`, options)
      .then(response => {
        if(!response.ok){
          alert_message = "Error updating an image!"
          alert = true
          tagValue = 'error'
          error = true
        }
        return response.text()
      })
      .then(response => {
        if(response == SUCCESS_UPDATE_MESSAGE_IMAGE) {
          alert_message = SUCCESS_UPDATE_MESSAGE_IMAGE
          alert = true
          tagValue = 'success'
          error = false
        }
        console.log(response)
      })
      .catch(err => {
        alert_message = "Error updating an image!"
        alert = true
        tagValue = 'error'
        error = true
        console.error(err)
      });

  }  
  $: teddyData, updateData();

</script>
<!-- Open the modal using ID.showModal() method -->
<!-- You can open the modal using ID.showModal() method -->
<dialog id="teddy_info_modal" class="modal">
  <div class="modal-box w-11/12 max-w-5xl">
    {#if teddyData != undefined}
    <h3 class="font-bold text-lg m-5">Information about {teddyData["name"]}!</h3>
    {#key teddyData["teddyBarcode"]}
     <Alert alert={alert} alert_message={alert_message} error={error} tagValue={tagValue}></Alert>
    {/key}
    <div class="w-11/12 flex">
        <div class="w-1/2 flex items-left flex-col">
          <PlushieImage imageId={teddyData["imageId"]}></PlushieImage>
          <label class="form-control w-full max-w-xs">
            <div class="label m-5">
              <span class="label-text">Drag and Drop or Select a new Image to replace:</span>
            </div>
            <input type="file" accept="image/*" class="file-input file-input-bordered file-input-info w-full max-w-xs m-5" bind:files on:change={() => updateImage()} required/>
            <div class="label">
            </div>
          </label>
        </div>
        <div class="w-1/2 flex items-right flex-col">
          <h1 class="text-left">Name: <b on:input={() => changed = true} contenteditable="true" bind:textContent={teddyData["name"]}>{teddyData["name"]}</b></h1>
          <h2 class="text-left">Received on: <b on:input={() => changed = true} contenteditable="true" bind:textContent={teddyData["dateReceived"]}>{teddyData["dateReceived"]}</b></h2>
          <h2 class="text-left">Gift From: <b on:input={() => changed = true} contenteditable="true" bind:textContent={teddyData["receivedFrom"]}>{teddyData["receivedFrom"]}</b></h2>
          <h2 class="text-left">City: <b on:input={() => changed = true} contenteditable="true" bind:textContent={teddyData["city"]}>{teddyData["city"]}</b></h2>

          <MapLibre
          style="https://basemaps.cartocdn.com/gl/positron-gl-style/style.json"
          class="relative aspect-[9/16] max-h-[70vh] w-full sm:aspect-video sm:max-h-full"
          standardControls
          >
              <DefaultMarker on:input={() => changed = true} lngLat={[teddyData["longitude"], teddyData["latitude"]]} draggable></DefaultMarker>
        </MapLibre>

          <h2 class="text-left">Description: <b on:input={() => changed = true} contenteditable="true" bind:textContent={teddyData["description"]}>{teddyData["description"]}</b></h2>
        </div>
      </div>
      <hr class="m-5">
      <h1>Actions: </h1>
      {#key teddyData["teddyBarcode"]}
        <DeleteButton teddyId={teddyData["teddyBarcode"]} imageId={teddyData["imageId"]}></DeleteButton>
      {/key}
    {/if}
    <div class="modal-action">
      <form method="dialog">
        <!-- if there is a button, it will close the modal -->
        <button class="btn">Close</button>
      </form>
    </div>
  </div>
</dialog>