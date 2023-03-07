<script>
// @ts-nocheck

    import "../tailwind.css"
    import Nav from '$lib/Nav.svelte'
    import Table from "$lib/Table.svelte";
    export let show = false
    function showTable(){
        show = !show
    }

</script>

<main data-theme="synthwave">
    <div class="container mx-auto" >
    <Nav />
    {#if !show}
    <div>
        <div class="hero min-h-screen bg-base-200">
            <div class="hero-content text-center">
                <div class="max-w-md">
                  <h1 class="text-5xl font-bold">Pagina de inicio</h1>
                  <button class="btn btn-active btn-primary" on:click={showTable}>Ver Autores</button>
                </div>
              </div>
        </div>
      </div>
      {:else}
      <button class="btn btn-active btn-primary" on:click={showTable}>Regresar</button>

        <Table />

    {/if}

    </div> 
</main>