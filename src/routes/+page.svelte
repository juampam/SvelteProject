<script>
// @ts-nocheck
  import { GraphQLClient } from 'graphql-request';
  import "../tailwind.css"
  import Nav from'../components/molecules/Nav.svelte'
	import Card from "../components/molecules/Card.svelte";
	import Table from '../components/molecules/Table.svelte';

  const client = new GraphQLClient('https://blog-backend-blush.vercel.app/graphql');

   let users = [];
   let books = [];
   let loader = "home"
   let totalposts = []
 //  let authorId


let resultado ;
let authorId 

function consultar() {
  const opcionQuery = `query {authors (options : {limit:12})  {rows {name,id}}}`
  const resultadoQuery = `query ($authorId:UUID!){posts(options:{where:{authorId:$authorId}}){rows{title,author{name}}}}`
  client.request(opcionQuery).then((opcionResult) => {
    console.log(opcionResult.authors.rows.length);
    for (let x = 0; x < opcionResult.authors.rows.length; x++) {
       authorId = opcionResult.authors.rows[x].id
       loader = "authors"

       console.log(authorId);
    }
    console.log(authorId);
    client.request(resultadoQuery,{authorId}).then((resultadoResult) => {
      resultado = resultadoResult;
      console.log(resultado.posts.rows);
      return resultado.posts.rows.length
    });
  });
}

 
  function getBooksArray(){
    const query = `query {posts (options : {limit:15}){rows{title,author{name},categories{name}}}}`
    client.request(query).then(data => {
        books = data.posts.rows
        loader = "libros"

        }).catch(error => {
          console.log("ERROR")
          console.error(error);
        }) 
  }
  function getUsers(){
    loader = "authors"
    const query = `query {authors (options : {limit:12})  {rows {name,id}}}`
    client.request(query).then(data => {
        users = data.authors.rows
        console.log(users)
        }).catch(error => {
          console.log("ERROR")
          console.error(error);
        }) 
  }
 



   const buttons = [
    {
      label: 'Autores',
      onClick: getUsers()
    },
    {
      label: 'Articulos',
      onClick: getBooksArray()
    },
    {
      label: 'Test',
      onClick: getUsers()
    },
  ]

 
</script>

<main >
    <div class="nav">
      <button class="btn btn-sm btn-primary" on:click={consultar}>Test</button>
      <button class="btn btn-sm btn-primary" on:click={getUsers}>Ver Por </button>
      <button class="btn btn-sm btn-primary" on:click={getBooksArray}>Titulos</button>

      {#if loader === "authors"}
      <div class=" container mx-auto gap-2 my-4">
      <div class="grid grid-cols-3 gap-2">
        {#each users as user}
        <Card 
        text={user.name}
        />
        {/each}
      </div>
    </div>
       {/if}
       {#if loader === "libros"}
        <Table items={books} /> 
        {/if}
        
    </div> 
         
</main>