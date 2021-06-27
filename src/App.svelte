<script>
	import Modal from './Modal.svelte';
	import AddForm from './AddForm.svelte';
	import EditForm from './EditForm.svelte';

	let contacts = [
		{ id: 1, first_name: 'John', last_name: 'Doe', address: '123 Main St.', city: 'Orange', state: 'CA', zip: '92112', email: 'johndoe@gmail.com', phone: '7145551234' },
		{ id: 2, first_name: 'Jane', last_name: 'Smith', address: '2843 19th Ave.', city: 'Mission', state: 'AZ', zip: '81124', email: 'janesmith@yahoo.com', phone: '7146232345' },
		{ id: 3, first_name: 'Adam', last_name: 'Klint', address: '13679 Cherry Ct.', city: 'Yellowstone', state: 'WY', zip: '41157', email: 'adamklint@bing.com', phone: '7148246789' },
	];

	let message = 'default';
	let showModal = false;
	$: count = Math.max.apply(Math, contacts.map(function(o) { return o.id; })) + 1;
	let editableContact = -1;

	let prefix = '';
	$: sortedData = prefix ? contacts.filter(contact => {
		return (contact.id === parseInt(prefix)) ||
		(contact.first_name.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.last_name.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.address.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.city.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.state.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.zip.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.email.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.phone.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase()));
	}) : contacts;

	let highlight = "id";
	const headers = Object.keys(contacts[0]);

	const sortByString = (header) => {
		sortedData = sortedData.sort((obj1, obj2) => {
			if(obj1[header] < obj2[header]){
				return -1;
			}
			else if (obj1[header] > obj2[header]){
				return 1;
			}
			return 0;
		})
		highlight = header;
	};

	const closeModal = () => {
		message = 'default';
		showModal = false;
	};

	const deleteContactButton = (id) => {
		contacts = contacts.filter((contact) => contact.id != id)
	};

	const addContactButton = () => {
		message = 'Add contact';
		contacts = contacts;
		showModal = true;
	};

	const editContactButton = (id) => {
		editableContact = contacts.find(contact => contact.id === id);
		console.log(editableContact);
		message = 'Edit contact';
		showModal = true;
	};

	const addContact = (e) => {
		const contact = e.detail;
		contacts = [contact, ...contacts];
		closeModal();
	};

	const editContact = (e) => {
		const contact = e.detail;
		contacts.map((obj) => {
			if(obj.id === contact.id){
				obj.first_name = contact.first_name;
				obj.last_name = contact.last_name;
				obj.address = contact.address;
				obj.city = contact.city;
				obj.state = contact.state;
				obj.zip = contact.zip;
				obj.email = contact.email;
				obj.phone = contact.phone;
			}
		});
		contacts = contacts;
		closeModal();
	};

</script>

<Modal {showModal} on:click={closeModal}>
	<h3>{message}</h3>

	{#if message === 'Add contact'}
		<AddForm {count} on:addContact={addContact}>
		</AddForm>
	{:else if message === 'Edit contact'}
		<EditForm {editableContact} on:editContact={editContact}>
		</EditForm>
	{/if}

</Modal>

<main>
	<h1>Contacts</h1>
	<center>
		<table id="contacts-table">

			<tr>
				{#each headers as header}
					<th class:highlighted={header === highlight} on:click={() => sortByString(header)}>{header.replace('_', ' ')}</th>
				{/each}
			</tr>

			{#each sortedData as contact}
				<tr>
					<td>{contact.id}</td>
					<td>{contact.first_name}</td>
					<td>{contact.last_name}</td>
					<td>{contact.address}</td>
					<td>{contact.city}</td>
					<td>{contact.state}</td>
					<td>{contact.zip}</td>
					<td>{contact.email}</td>
					<td>{contact.phone}</td>
					<button on:click={() => editContactButton(contact.id)}>Edit</button>
					<button on:click={() => deleteContactButton(contact.id)}>Delete</button>
				</tr>
			{/each}

		</table>
	</center>
	<button on:click={addContactButton}>Add</button>
	<input type="text" placeholder="Filter" bind:value={prefix}>
</main>

<style>
	.highlighted{
		color: orangered;
	}
	th {
		text-transform: uppercase;
		cursor: pointer;
	}
	th, td {
		text-align: left;
		padding: 15px;
	}
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
	button{
		width: 100px;
		margin: 0 5px;
		border-radius: 6px;
	}
	h3{
		color: orangered;
	}
	input[type=text] {
        margin: 5px 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 6px;
        box-sizing: border-box;
    }
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>