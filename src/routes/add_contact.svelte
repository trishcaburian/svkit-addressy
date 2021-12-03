<script>
    import supabase from "$lib/db";

    // contact
    let contact;
    let firstname;
    let lastname;
    let phone = '';
    let person_img;
    let email = '';

    // address
    let contact_address;
    let address;
    let city;
    let state;
    let zip;
    let country_id;

    let uploaded;

    async function saveData() {
        contact_address = {
            address: address,
            city: city,
            state: state,
            zip: zip,
            country_id: country_id
        };

        // save to db, respond with address id
        await supabase
            .from('address')
            .insert(contact_address).then((data) => {
                let address_data = data.data[0];
                addPerson(address_data.id);
            });
    }

    async function addPerson(address_id) {
        contact = {
            firstname: firstname,
            lastname: lastname,
            phone: phone,
            email: email,
            address_id: address_id,
            avatar: person_img
        };

        // save to db, redirect to home
        await supabase
            .from('contact')
            .insert(contact)
            .then(() => {
                window.location.href = '/';
            });
    }

    async function getCountries() {
        const { data, error } = await supabase
            .from('countries')
            .select()
        if (error) throw new Error(error.message)

        return data
    }

    function uploadImg(e) {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(image);
        reader.onload = e => {
            person_img = e.target.result
        };
    }
</script>

<div class="p-5 flex justify-center flex-grow">
    <form class="w-full max-w-lg shadow-lg p-3.5 bg-white" on:submit|preventDefault={saveData}>
        <!-- UPLOAD AVATAR -->
        <div class="flex flex-wrap -mx-3 mb-6">
            <div class="w-full px-3">
                <label class="field_label" for="person_pic">Upload Image</label>
                {#if person_img}
                    <img class="avatar mx-auto" src="{person_img}" alt="d" />
                {:else}
                    <img class="avatar mx-auto" src="https://cdn4.iconfinder.com/data/icons/small-n-flat/24/user-alt-512.png" alt="" />
                {/if}
                <div id="person_pic" class="align-center text-center p-3.5 hover:bg-red-400 bg-blue-500 text-white h-12" on:click={()=>{uploaded.click();}}>Choose Image</div>
                <input style="display:none" type="file" accept=".jpg, .jpeg, .png" on:change={(e)=>uploadImg(e)} bind:this={uploaded} >
            </div>
        </div>

        <!-- NAME -->
        <div class="flex flex-wrap -mx-3 mb-6">
            <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
                <label class="field_label" for="firstname">
                    First Name
                </label>
                <input type="text" class="regular_field" id="firstname" name="firstname" bind:value={firstname} placeholder="First Name" required/>
                <p id="fn_required" class="text-red-500 text-xs italic" hidden>Please fill out this field.</p>
            </div>
            <div class="w-full md:w-1/2 px-3">
                <label class="field_label" for="lastname">
                    Last Name
                </label>
                <input type="text" class="regular_field" id="lastname" name="lastname" bind:value={lastname} placeholder="Last Name" required/>
                <p id="ln_required" class="text-red-500 text-xs italic" hidden>Please fill out this field.</p>
            </div>
        </div>

        <!-- ADDRESS -->
        <div class="flex flex-wrap -mx-3 mb-3">
            <div class="w-full mb-3 px-3">
                <label class="field_label" for="address_1">Address</label>
                <input class="regular_field" type="text" id="address_1" bind:value={address} placeholder="Address 1"/>
            </div>
            <div class="w-full md:w-1/3 px-3 mb-3">
                <label class="field_label" for="country">City</label>
                <input class="regular_field" type="text" id="city" bind:value={city} placeholder="City"/>
            </div>
            <div class="w-full md:w-1/3 px-3 mb-3">
                <label class="field_label" for="state">State</label>
                <input class="regular_field" type="text" id="state" bind:value={state} placeholder="State"/>
            </div>
            <div class="w-full md:w-1/3 px-3 mb-3">
                <label class="field_label" for="country">ZIP Code</label>
                <input class="regular_field" type="text" id="zip" bind:value={zip} placeholder="ZIP Code"/>
            </div>
            <div class="w-full mb-3 px-3">
                <label class="field_label" for="country">Country</label>
                <select class="dropdown_field" id="country" bind:value={country_id}>
                    {#await getCountries()}
                    {:then data}
                        <option value="None">Please Select</option>
                        {#each data as country}
                            <option value={country.id}>{country.name}</option>
                        {/each}
                    {/await}
                </select>

            </div>
        </div>

        <!-- PHONE + EMAIL-->
        <div class="flex flex-wrap -mx-3 mb-3">
            <div class="w-full mb-3 px-3">
                <label class="field_label" for="phone">Phone Number</label>
                <input class="regular_field" type="tel" id="phone" bind:value={phone} placeholder="Phone Number"/>
            </div>
        </div>
        <div class="flex flex-wrap -mx-3 mb-6">
            <div class="w-full mb-3 px-3">
                <label class="field_label" for="email">Email Address</label>
                <input class="regular_field" type="tel" id="email" bind:value={email} placeholder="Email Address"/>
            </div>
        </div>

        <button type="submit" class="form_button">Save</button>
    </form>
</div>