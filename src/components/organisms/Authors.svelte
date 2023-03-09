<script>
    // @ts-nocheck
      import { GraphQLClient } from 'graphql-request';
      import "../tailwind.css"
      import Nav from'../components/molecules/Nav.svelte'
      import Card from "../components/molecules/Card.svelte";
      import Table from '../components/molecules/Table.svelte';
      const client = new GraphQLClient('https://blog-backend-blush.vercel.app/graphql');
       
    
       let getAuthors = () => {
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
          }
        
      
    </script>
    
    <main >
        <div>
          <div class=" container mx-auto grid grid-cols-3 gap-2 my-4">
            {#each books as book}
            <Table
            title={book.title}
            author={book.author.name}
            />
            {/each}
       
          </div>
        </div> 
    </main>