<script>
	import { page } from '$app/stores';
	import { GraphQLClient } from 'graphql-request';
	import Card from '../../../components/molecules/Card.svelte';
	import { onMount } from 'svelte';

	const client = new GraphQLClient('https://blog-backend-blush.vercel.app/graphql');

	let posts = [];
	let author = {};
	let loading = true;

	const getAuthor = async (id) => {
		const query = `query($id: UUID!){
            author(id:$id){
                id
                name
            }
        }`;

		let response = await client.request(query, { id });
		console.log(response);
		loading = false;
		return response.author;
	};

	const getPosts = async (id) => {
		const query = `# Write your query or mutation here
            query($id: UUID){
            posts(options:{
                where:{
                authorId:$id
                }
            }){
                rows{
                id
                title
                reactions
                categories{
                id
                    name
                }
                }
                length
            }
        }`;

		let response = await client.request(query, { id });
		console.log(response.posts);
		return response.posts.rows;
	};

	onMount(async () => {
		author = await getAuthor($page.params.id);
		posts = await getPosts($page.params.id);
	});
</script>

{#if loading}
	<div class="h-screen w-screeen flex items-center justify-center text-3xl font-bold">
		Cargando...
	</div>
{:else}
	<div class="container mx-auto p-8">
		<Card text={author?.name || 'name'} />
		<div class="overflow-x-auto mt-8">
			<table class="table w-full">
				<!-- head -->
				<thead>
					<tr>
						<th>Id</th>
						<th>Titulo</th>
						<th>Reacciones</th> 
						<th>Categorías</th>
					</tr>
				</thead>
				<tbody>
                    {#each posts as post}
                        <tr>
                            <th>{post.id}</th>
                            <td>{post.title}</td>
                            <td>
                                <span class="text-lg">{post.reactions}</span>
                            </td>
                            <td class="flex gap-2">
                                {#each post?.categories || [] as category}
                                    <div class="badge badge-primary">{category.name}</div>
                                {/each}
                            </td>
                        </tr>
                    {/each}
				</tbody>
			</table>
		</div>
	</div>
{/if}
