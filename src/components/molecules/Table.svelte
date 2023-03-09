<script>
	import { each } from "svelte/internal";
import Badge from "../atoms/Badge.svelte";
export let items



</script>
<div class="overflow-x-auto">
 
    <table class="table table-compact w-full">
      <thead>
        <tr>
          <th>No.</th>
          <th>Titulo</th>
          <th>Autor</th>
          <th>Categorias</th>

        </tr>
      </thead>
      <tbody>
        {#each items as item, i}
        <tr class="active">
          <th>{i + 1}</th>
          <td>{item.title}</td>
          <td>{item.author.name}</td>
          <td >
            <div class="flex between">
              {#each item.categories as category}
              <Badge val={category.name}/>
              {/each}
            </div>
          </td>
        </tr>
        {/each}
      </tbody>
    </table>
    
  </div>

