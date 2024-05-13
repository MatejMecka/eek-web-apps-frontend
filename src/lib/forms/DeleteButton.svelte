<script lang="ts">
    export let teddyId: String;
    export let imageId: String;
    let checked = false;
    let deleted = false;
    let alert_message = "Succesfully deleted!"

    console.log(teddyId);

    const handleClick = function(){
        console.log("CLICKED")
        if(!checked){
            return
        }
        const options = {method: 'DELETE'};
        fetch(`${import.meta.env.VITE_API_URL}teddybear/${teddyId}`, options)
        .then(response => {response.text()})
        .then(response => {
            console.log(response)
            fetch(`${import.meta.env.VITE_API_URL}images/${teddyId}`, options)
            .then(response => {response.text()})
            .then(response2 => {
                console.log(response2)
                deleted = true;

            }).catch(err => console.error(err));

        })
        .catch(err => console.error(err));
    }

</script>
{#key teddyId}
    {#if deleted}
        <div role="alert" class="alert alert-success">
            <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current shrink-0 h-6 w-6" fill="none" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
            <span>Succesfully deleted!</span>
        </div>
    {/if}
    {#if !deleted}
        <button class="btn btn-error m-5" on:click={handleClick}>Delete</button>
        <div class="form-control">
            <label class="cursor-pointer label">
                <span class="label-text">Yes, I confirm the deletion!</span>
                <input type="checkbox" bind:checked class="checkbox checkbox-error" />
            </label>
        </div>
    {/if}
{/key}
