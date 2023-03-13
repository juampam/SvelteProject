<script>
    export let indexvar
    function next(){
      indexvar= indexvar+1
    }
    function prev(){
      if (indexvar <= 1) {
        indexvar = 1
        console.log(indexvar)

      }else{
        indexvar = indexvar - 1
        console.log(indexvar)
      }
    }
</script>
<div class="btn-group grid grid-cols-2">
  <button on:click={prev()}   class="btn btn-xs btn-outline">Previous page</button>
  <button on:click={next()} class="btn btn-xs btn-outline">Next</button>
</div>