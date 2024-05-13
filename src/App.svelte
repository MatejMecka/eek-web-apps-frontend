<script lang="ts">
  import Search from './lib/Search.svelte';
  import Gallery from './lib/Gallery.svelte';
  import Fab from './lib/FAB.svelte';
  import TeddyInfoModal from './lib/modals/TeddyInfoModal.svelte';

  let selectedResponse = undefined;
</script>

<main>
  <div class="search">
    <h1>Malishan.net</h1>
    <Search bind:selectedResponse></Search>
  </div>
  <Gallery bind:selectedResponse></Gallery>
  <Fab></Fab>
  <TeddyInfoModal teddyData={selectedResponse}></TeddyInfoModal>
</main>

<style global>
  @import 'tailwindcss/base';
  @import 'tailwindcss/components';
  @import 'tailwindcss/utilities';
</style>
