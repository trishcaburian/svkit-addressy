<!--<h1>Welcome to SvelteKit</h1>-->
<!--<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>-->
<script>
   import supabase from "$lib/db";

   async function getData() {
       const { data, error } = await supabase
           .from('countries')
           .select()
       if (error) throw new Error(error.message)

       return data
   }
</script>

{#await getData()}
    <p>Fetching data</p>
{:then data}
    {#each data as country}
        <li>{country.name}</li>
    {/each}
{:catch error}
    <p>Error</p>
    <pre>{error}</pre>
{/await}
