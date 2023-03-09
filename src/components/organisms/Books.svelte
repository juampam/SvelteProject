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
       let loader 
       
       const buttons = [
        {
          label: 'Autores',
          onClick: () =>  {
            const query = `
            query getusers {
                authors (options : {limit:12})  {
                    rows {
                        name
                    }
                }
            }
            `
            client.request(query).then(data => {
              loader = "authors"
              users = data.authors.rows
    
            }).catch(error => {
                console.log("ERROR")
                console.error(error);
            });
          },
        },
        {
          label: 'Articulos',
          onClick: () => {
            const query = `
            query  {
              posts{
                  rows{
                  title,author{name}
                }
              }
            }`
            client.request(query).then(data => {
              books = data.posts.rows
              loader = "libros"
              console.log(books[0].title)
            }).catch(error => {
                console.log("ERROR")
                console.error(error);
            
            })
          },
        },
      ]; 
       
    </script>
    
    <main >
        <div>
          <Nav buttons={buttons} />
          <div class=" container mx-auto grid grid-cols-3 gap-2 my-4">
          {#if loader === "authors"}
            {#each users as user}
            <Card
            text={user.name}
            />
            {/each}
           {/if}
           {#if loader === "libros"}
            {#each books as book}
            <Table
            title={book.title}
            author={book.author.name}
            />
            {/each}
            {/if}
          </div>
        </div> 
    </main>