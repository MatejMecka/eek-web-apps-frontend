<script lang="ts">
    import { faQuestion } from "@fortawesome/free-solid-svg-icons";
    import Fa from "svelte-fa";

    export let imageId;

    let image = ""

    const options = {method: 'GET'};

    const fetchImage = (async () => {
        console.log()
        fetch(`http://localhost:8080/api/images/${imageId}`, options).then(response => response.json())
        .then(response => {
            console.log(response)
            image = `data:image/png;base64, ${response['fileContents']}`
            return response
        })
        .catch(err => console.error(err));
    })

</script>

{#key imageId}
    {#await fetchImage()}
        <span class="loading loading-spinner loading-lg text-primary"></span>
    {:then data}
        <figure class="m-2">
            <img src={image} alt="Album"/>
        </figure>
    {:catch error}
        <Fa icon={faQuestion}/>
    {/await}
{/key}