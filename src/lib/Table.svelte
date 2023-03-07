<script>
    import { createEventDispatcher } from 'svelte';
    import { onMount, setContext } from 'svelte';
    import { GraphQLClient } from 'graphql-request';

    const dispatch = createEventDispatcher();
  
    function handleClick() {
      dispatch('show-component');
    }

    const client = new GraphQLClient('https://blog-backend-blush.vercel.app/graphql');
    let users = [];
    setContext('users', users);

    function getusers() {
        const query = `
        query getusers  {
            authors  {
                rows {
                    name, id
                }
            }
        }
        `
        client.request(query).then(data => {
    
          const usersJson = JSON.stringify(data); // Convert object to JSON string
         // console.log(usersJson)
          var convert = JSON.parse(usersJson)
          users = convert.authors.rows
          console.log(users.length)
          
     /*     for (let i = 0; i < users.length; i++) {
                  console.log(users) 
          }*/
          
        }).catch(error => {
            console.log("ERROR")
            console.error(error);
        });
    }



  </script>
<button class="btn btn-active btn-secundary" on:click={getusers}>Ver todos</button>

<div class="overflow-x-auto">
    <table class="table w-full">
      <!-- head -->
      <thead>
        <tr>
          <th>Id</th>
          <th>Nombre</th>
        </tr>
      </thead>
      <tbody>
        {#each users as user}

        <!-- row 1 -->
        <tr>
          <td>{user.id}</td> 
          <td>{user.name}</td>
        </tr>
        {/each}
      </tbody>
    </table>
  </div>