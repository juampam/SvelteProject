<script>
    import Avatar from "../atoms/Avatar.svelte";
	  export let text
</script>


<div class="card w-96 bg-neutral text-neutral-content">
    <div class="card-body items-center text-center">
      <Avatar src="https://i.stack.imgur.com/l60Hf.png" size="80px" />
      <h2 class="card-title">
        {text}
    </h2>
    </div>
    <div class="card-actions justify-center">
      <h1>Hola</h1>
    </div>
  </div>