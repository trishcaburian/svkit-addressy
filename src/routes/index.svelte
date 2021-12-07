<!--<h1>Welcome to SvelteKit</h1>-->
<!--<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>-->
<script>
   import supabase from "$lib/db";

   let search_text;

   async function getData() {
       const { data, error } = await supabase
           .from('contact')
           .select()
       if (error) throw new Error(error.message)

       return data
   }

   function deleteContact() {
       // future function
   }
</script>
<div class="p-5 flex justify-center flex-grow">
    <div class="md:w-3/4 bg-white">
<!--        <div class="w-full sticky top-0 bg-white place-items-center">-->
<!--            <div class="p-5">-->
<!--                <input type="text" class="regular_field w-full" bind:value={search_text} placeholder="Search"/>-->
<!--            </div>-->
<!--        </div>-->
        <div class="mt-1">
            {#await getData()}
                <p>Fetching data</p>
            {:then data}
                {#each data as contact}
                    <hr class="w-full">
                    <div class="w-full block flex-grow px-4 sm:flex sm:items-center sm:w-auto">
                        {#if contact.avatar !== null}
                            <img class="avatar_mini" src={contact.avatar} alt="d" />
                        {:else}
                            <img class="avatar_mini flex flex-nowrap" src="https://cdn4.iconfinder.com/data/icons/small-n-flat/24/user-alt-512.png" alt="" />
                        {/if}
                        <div class="font-bold text-xl m-5 hover:underline cursor-pointer" on:click={() => (window.location.href = "/contact/"+contact.id)}>
                            {contact.firstname} {contact.lastname}
                        </div>
                        <div class="object-right text-sm">
                            {contact.phone}
                        </div>
                    </div>
                {/each}
            {:catch error}
                <p>Error</p>
                <pre>{error}</pre>
            {/await}
        </div>
    </div>
</div>