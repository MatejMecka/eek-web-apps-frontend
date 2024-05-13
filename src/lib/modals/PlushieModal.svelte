<script lang="ts">
    import PlushieForm from "../forms/PlushieForm.svelte";

</script>
<!-- Open the modal using ID.showModal() method -->
<dialog id="add_plushie_modal" class="modal">
  <div class="modal-box w-11/12 max-w-5xl">
    <h3 class="font-bold text-lg">Create New Plushie</h3>
    <PlushieForm></PlushieForm>
  </div>
  <form method="dialog" class="modal-backdrop">
    <button>close</button>
  </form>
</dialog>