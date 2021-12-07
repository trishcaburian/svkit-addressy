<!-- Show selected contact -->
<script>
    import { fade } from 'svelte/transition';
    import { page } from '$app/stores';
    import supabase from "$lib/db.js";
    let contactId = $page.params.contact;

    console.log(contactId);

    async function getContact() {
        const { data, error } = await supabase
        .from('contact')
        .select('*,  address!inner(*, countries:country_id(*))')
        .eq('contact.id', contactId)
        .limit(1)
        .single();

        if (error) throw new Error(error.message)

        return data;
    }
</script>

<div class="flex justify-center">

    {#await getContact()}
        <div class="w-full max-w-lg shadow-lg p-3.5 bg-white mt-5" in:fade>
            loading...
        </div>
    {:then selectedContact}
        <div class="w-full max-w-lg shadow-lg p-3.5 bg-white mt-5" in:fade>
            <div>
                {#if selectedContact.avatar !== ''}
                    <img class="avatar mx-auto" src="{selectedContact.avatar}" alt="d" />
                {:else}
                    <img class="avatar mx-auto flex flex-nowrap" src="https://cdn4.iconfinder.com/data/icons/small-n-flat/24/user-alt-512.png" alt="" />
                {/if}
            </div>
            <div class="mb-4">
                <label class="field_label">Name</label>
                {selectedContact.firstname} {selectedContact.lastname}
            </div>
            <div class="mb-4">
                <label class="field_label">Address</label>
                {#if selectedContact !== undefined}
                    {selectedContact.address.address}, {selectedContact.address.city}, {selectedContact.address.state},
                    {selectedContact.address.zip}, {selectedContact.address.countries.name}
                {/if}
            </div>
            <div class="mb-4">
                <label class="field_label">Phone Number</label>
                {selectedContact.phone}
            </div>
        </div>
    {/await}

</div>