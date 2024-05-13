<script lang="ts">
    import { faBarcode, faCity, faPen } from '@fortawesome/free-solid-svg-icons';
    import { DateInput } from 'date-picker-svelte'
    import Fa from 'svelte-fa';
    import { MapLibre, DefaultMarker } from 'svelte-maplibre';
    import Alert from '../Alert.svelte';

    let alert_message: string | undefined = undefined
    let RESPONSE_SUCCESS_MESSAGE = "Teddy Bear Saved Successfully!"
    const DB_ERROR_MESSAGE = "There was something wrong when executing in the database."

    let lngLat = [0, 0]
	  let date = new Date()

    let barcode: String;
    let name: String;
    let received: String;
    let city: String;
    let desc: String;
    let files: (string | Blob)[];

    let alertStatus = false;
    let error: boolean;
    let tagValue = "success"

    const createPlushie = function () {
      const form = new FormData();
      form.append("image", files[0]);

      if (form.has('image') == false) {
        return
      }

      const options = {
        method: 'POST',
        body: form
      };

      console.log(options);
      fetch(`${import.meta.env.VITE_API_URL}images/create`, options)
        .then(response => {
          if(!response.ok){
              alertStatus = true;
              error = true;
              alert_message = "There was an error creating an image!"
              tagValue = "error"
          }
          return response.json()
        })
        .then(response => {
          console.log(response)

          const options_teddy = {
              method: 'POST',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({
                "teddyBarcode": `${barcode}`,
                "name": name,
                "dateReceived": date,
                "receivedFrom": received,
                "city": city,
                "description": desc,
                "latitude": lngLat[1],
                "longitude": lngLat[0],
                "imageId": response
              })
            }

            fetch(`${import.meta.env.VITE_API_URL}teddybear/create`, options_teddy)
            .then(response => {
              if(!response.ok) {
                alertStatus = true;
                error = true;
                alert_message = "There was an error creating a plushie! Retry again later."
                tagValue = "error"
              }
              if(response.status == 409) {
                alertStatus = true;
                error = true;
                alert_message = "A Teddy Bear with that Barcode already exists!"
                tagValue = "error"
              }
              return response.text()
            })
            .then(response => {
              console.log("RESPONSE 2")
              console.log(response)

              if(response == RESPONSE_SUCCESS_MESSAGE){
                alertStatus = true;
                error = false;
                alert_message = "Saved Succesfully!"
                tagValue = "success"
              } else if(response == DB_ERROR_MESSAGE) {
                  alert_message = DB_ERROR_MESSAGE
                  alertStatus = true
                  tagValue = 'error'
                  error = true
                }
              console.log(response);
            }).catch(err => {
              alertStatus = true;
              error = true;
              alert_message = "There was an error connecting to the server!"
              tagValue = "error"
              console.log(err)
            })


        })
        .catch(err => {
          alertStatus = true;
          error = true;
          alert_message = "There was an error connecting to the server!"
          tagValue = "error"
          console.error(err)
        });
    }


</script>
  <Alert alert={alertStatus} alert_message={alert_message} error={error} tagValue={tagValue}></Alert>
<form onsubmit="return false;">
    <label class="input input-bordered flex items-center gap-2 m-5">
      <Fa icon={faBarcode} />
        <input type="number" class="grow" placeholder="Barcode" bind:value={barcode} required/>
      </label>
      <label class="input input-bordered flex items-center gap-2 m-5">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="w-4 h-4 opacity-70"><path d="M8 8a3 3 0 1 0 0-6 3 3 0 0 0 0 6ZM12.735 14c.618 0 1.093-.561.872-1.139a6.002 6.002 0 0 0-11.215 0c-.22.578.254 1.139.872 1.139h9.47Z" /></svg>
        <input type="text" class="grow" placeholder="Name" bind:value={name} required />
      </label>

      <DateInput bind:value={date} />

      <label class="input input-bordered flex items-center gap-2 m-5">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="w-4 h-4 opacity-70"><path d="M8 8a3 3 0 1 0 0-6 3 3 0 0 0 0 6ZM12.735 14c.618 0 1.093-.561.872-1.139a6.002 6.002 0 0 0-11.215 0c-.22.578.254 1.139.872 1.139h9.47Z" /></svg>
        <input type="text" class="grow" placeholder="Received From" bind:value={received} required />
      </label>
      <label class="input input-bordered flex items-center gap-2 m-5">
        <Fa icon={faCity}/>
        <input type="text" class="grow" placeholder="City" bind:value={city} required />
      </label>
      
      <div class="m-5">
        <MapLibre
        style="https://basemaps.cartocdn.com/gl/positron-gl-style/style.json"
        class="relative aspect-[9/16] max-h-[70vh] w-full sm:aspect-video sm:max-h-full"
        standardControls
        >
            <DefaultMarker bind:lngLat={lngLat} draggable></DefaultMarker>
      </MapLibre>
    </div>

    <div class="flex flex-row m-5">
        <textarea class="textarea textarea-bordered input input-ghost w-full max-w-xs" placeholder="Description" rows="10" bind:value={desc} required></textarea>
    </div>

    <input type="file" accept="image/*" class="file-input file-input-bordered file-input-info w-full max-w-xs m-5" bind:files required/>
    <br>
    <button class="btn btn-success" on:click={createPlushie}>Submit</button>
</form>