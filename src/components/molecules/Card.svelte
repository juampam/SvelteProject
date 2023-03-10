<script>
    import Avatar from "../atoms/Avatar.svelte";
	  export let text
    export let length = 0
</script>


<div class="card bg-neutral text-neutral-content ">
    <div class="card-body items-center text-center">
      <Avatar src="https://i.stack.imgur.com/l60Hf.png" size="80px" />
      <h2 class="card-title">
        {text}
    </h2>
    </div>
    <div class="card-actions justify-center p-4">
      <h1>{length || 'No existen artículos'}</h1>
    </div>
  </div>