<script lang="ts">
    import { faPlus } from "@fortawesome/free-solid-svg-icons";
    import Fa from "svelte-fa";
    import PlushieModal from "./modals/PlushieModal.svelte";
</script>
<div class="fixed bottom-4 right-4">
    <button class="btn bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-full shadow-lg" onclick="add_plushie_modal.showModal()">
       <Fa icon={faPlus} /> 
       Add Plushie
    </button>
</div>
  
<PlushieModal></PlushieModal>