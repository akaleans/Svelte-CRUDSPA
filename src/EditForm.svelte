<script>
    // Import our event dispatcher used to send data to our App component.
    import { createEventDispatcher } from 'svelte';

    let dispatch = createEventDispatcher();

    // Our contact to be edited, sent by App component.
    export let editableContact;

    /*
    On submitting our form, creates the new contact to replace the current contact at contacts.id
    and dispatches our event back to App component.
    */
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
    };
</script>

<!-- Default Form -->
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
    button:hover{
		font-family: 'Montserrat', sans-serif;
		color: #121212;
		background: white;
		width: 150px;
		margin: 36px 5px 36px 0;
	}
	button{
		border: 2px solid #121212;
		transition-property: background, color;
		transition-duration: 0.25s;
		font-family: 'Montserrat', sans-serif;
		color: white;
		background: #121212;
		width: 150px;
		margin: 36px 5px 36px 0;
	}
    input[type=text] {
        font-family: 'Montserrat', sans-serif;
        width: 100%;
        padding: 10px 10px;
        margin: 5px 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 6px;
        box-sizing: border-box;
    }
</style>