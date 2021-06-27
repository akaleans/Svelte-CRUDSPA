<script>
    import { createEventDispatcher } from 'svelte';

    let dispatch = createEventDispatcher();

    export let editableContact;

    const onSubmit = () => {
        const contact = {
            id: editableContact.id,
            first_name: editableContact.first_name,
            last_name: editableContact.last_name,
            address: editableContact.address,
            city: editableContact.city,
            state: editableContact.state,
            zip: editableContact.zip,
            email: editableContact.email,
            phone: editableContact.phone
        };
        dispatch('editContact', contact);
        console.log(contact);
    };
</script>

<form on:submit|preventDefault={onSubmit}>
    <input type="text" placeholder="First Name" bind:value={editableContact.first_name}>
    <input type="text" placeholder="Last Name" bind:value={editableContact.last_name}>
    <input type="text" placeholder="Street Address" bind:value={editableContact.address}>
    <input type="text" placeholder="City" bind:value={editableContact.city}>
    <input type="text" placeholder="State" bind:value={editableContact.state}>
    <input type="text" placeholder="Zip Code" bind:value={editableContact.zip}>
    <input type="text" placeholder="Email" bind:value={editableContact.email}>
    <input type="text" placeholder="Phone Number" bind:value={editableContact.phone}>
    <button>Finish Edit</button>
</form>

<style>
    button{
        width: 150px;
        border-radius: 6px;
    }
    input[type=text] {
        width: 100%;
        padding: 10px 10px;
        margin: 5px 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 6px;
        box-sizing: border-box;
    }
</style>