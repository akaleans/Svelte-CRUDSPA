<script>
    // Import our event dispatcher used to send data to our App component.
    import { createEventDispatcher } from 'svelte';

    let dispatch = createEventDispatcher();

    // Bringing in our current "id" count and creating temporary variables.
    export let count;
    let first_name;
    let last_name;
    let address;
    let city;
    let state;
    let zip;
    let email;
    let phone;

    /*
    On submitting our form, creates the new contact to be added to our contacts list, increments count
    and dispatches our event back to our App component.
    */
    const onSubmit = () => {
        const contact = {
            id: count,
            first_name: first_name,
            last_name: last_name,
            address: address,
            city: city,
            state: state,
            zip: zip,
            email: email,
            phone: phone
        };
        count = count + 1
        dispatch('addContact', contact);
    };
</script>

<!-- Default form -->
<form on:submit|preventDefault={onSubmit}>
    <input type="text" placeholder="First Name" bind:value={first_name}>
    <input type="text" placeholder="Last Name" bind:value={last_name}>
    <input type="text" placeholder="Street Address" bind:value={address}>
    <input type="text" placeholder="City" bind:value={city}>
    <input type="text" placeholder="State" bind:value={state}>
    <input type="text" placeholder="Zip Code" bind:value={zip}>
    <input type="text" placeholder="Email" bind:value={email}>
    <input type="text" placeholder="Phone Number" bind:value={phone}>
    <button>Add</button>
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