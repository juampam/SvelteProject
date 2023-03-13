<script>
    import { GraphQLClient } from "graphql-request";
    import { onMount } from 'svelte';   
    import Nav from "../../components/molecules/Nav.svelte";
    import Table from "../../components/molecules/Table.svelte";
    import Pagination from "../../components/molecules/Pagination.svelte";
    const client = new GraphQLClient('https://blog-backend-blush.vercel.app/graphql');
    let books = [];
    let queryv = 0

  function handleClick() {
    queryv -= 1;
    console.log(queryv)
    const query = `query($queryv: Int) {posts (options : {page:$queryv}){rows{title,author{name},categories{name}}}}`
     client.request(query,{queryv}).then(data => {
        books = data.posts.rows
        return books
        }).catch(error => {
          console.log("ERROR")
          console.error(error);
        }) 
  }
   function getusers() {
    queryv += 1;
    console.log(queryv)
    const query = `query($queryv: Int) {posts (options : {page:$queryv}){rows{title,author{name},categories{name}}}}`
     client.request(query,{queryv}).then(data => {
        books = data.posts.rows
        return books
        }).catch(error => {
          console.log("ERROR")
          console.error(error);
        }) 
  }

  
  onMount(async () => {
   getusers()
	});
</script>
    <Nav/>
    <Table items={books} />
   <div class="flex justify-center mt-8">
    <div class="btn-group">
      {#if queryv >= 2 }
      <button on:click={handleClick} class="btn btn-xs btn-outline">Previous page</button>
      <span class="px-4">{queryv}</span>

      {/if}
     <button on:click={getusers} class="btn btn-xs btn-outline">Next page</button>
    </div>
   </div>
    
