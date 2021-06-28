<script>
    import { createEventDispatcher } from 'svelte';

    let dispatch = createEventDispatcher();

    export let count;
    let first_name;
    let last_name;
    let address;
    let city;
    let state;
    let zip;
    let email;
    let phone;

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
		width: 100px;
		margin: 0 5px;
	}
	button{
		border: 2px solid #121212;
		transition-property: background, color;
		transition-duration: 0.25s;
		font-family: 'Montserrat', sans-serif;
		color: white;
		background: #121212;
		width: 100px;
		margin: 0 5px;
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