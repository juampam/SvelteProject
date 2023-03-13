<script>
	// @ts-nocheck
	import { GraphQLClient } from 'graphql-request';
	import Card from '../../components/molecules/Card.svelte';
	import { onMount } from 'svelte';
	import Nav from '../../components/molecules/Nav.svelte';
	const client = new GraphQLClient('https://blog-backend-blush.vercel.app/graphql');

	let users = [];
	let books = [];
	let loader = 'home';
	let totalposts = [];
	//  let authorId

	let resultado;
	let authorId;

	function consultar() {
		const opcionQuery = `query {authors (options : {limit:12})  {rows {name,id}}}`;
		const resultadoQuery = `query ($authorId:UUID!){posts(options:{where:{authorId:$authorId}}){rows{title,author{name}}}}`;
		client.request(opcionQuery).then((opcionResult) => {
			console.log(opcionResult.authors.rows.length);
			for (let x = 0; x < opcionResult.authors.rows.length; x++) {
				authorId = opcionResult.authors.rows[x].id;
				loader = 'authors';

				console.log(authorId);
			}
			console.log(authorId);
			client.request(resultadoQuery, { authorId }).then((resultadoResult) => {
				resultado = resultadoResult;
				console.log(resultado.posts.rows);
				return resultado.posts.rows.length;
			});
		});
	}

	function getBooksArray() {
		const query = `query {posts (options : {limit:15}){rows{title,author{name},categories{name}}}}`;
		client
			.request(query)
			.then((data) => {
				books = data.posts.rows;
				loader = 'libros';
			})
			.catch((error) => {
				console.log('ERROR');
				console.error(error);
			});
	}
	async function getUsers() {
		loader = 'authors';
		const query = `query {authors (options : {limit:12})  {rows {name,id}}}`;
		const data = await client.request(query)
        users = data.authors.rows;
        loading = false        
			// .then((data) => {
			// 	users = data.authors.rows;
			// 	console.log(users);
			// })
			// .catch((error) => {
			// 	console.log('ERROR');
			// 	console.error(error);
			// });
	}

  const getPosts = async(id) => {
		const query = `query($id: UUID){
      posts(options:{
        where:{
          authorId:$id
        }
      }){
        length
      }
    }`;

    let response = await client.request(query, {id})
    console.log(response.posts)
    return response.posts.length
    
		// client.request(query).then((data) => {
		// 		users = data.authors.rows;
		// 		console.log(users);
		// 	})
		// 	.catch((error) => {
		// 		console.log('ERROR');
		// 		console.error(error);
		// 	});
	}
  
let loading = true
onMount(() => {
    getUsers()
})

</script>

<Nav/>
{#if loading}
    <div class="h-screen w-screeen flex items-center justify-center text-3xl font-bold">
        Cargando...
    </div>
{:else}
<div class="container mx-auto p-8">
    <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-4 gap-4">
    {#each users as user}
        <a href={`/autores/${user.id}`}>
            {#await getPosts(user.id)}
            <Card text={user.name} />
            {:then number}
            <Card text={user.name} length={number + ' artículos'} />
            {:catch error}
            <p style="color: red">{error.message}</p>
            {/await}
        </a>
    {/each}
    </div>
</div>

{/if}