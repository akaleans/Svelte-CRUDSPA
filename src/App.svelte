<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700&display=swap" rel="stylesheet">
</svelte:head>

<script>
	import Modal from './Modal.svelte';
	import AddForm from './AddForm.svelte';
	import EditForm from './EditForm.svelte';

	let contacts = [{"id":1,"first_name":"Wyn","last_name":"Dymick","address":"2899 Memorial Park","state":"MA","city":"Boston","zip":"74263","email":"wdymick0@github.com","phone":"617-250-4771"},
		{"id":2,"first_name":"Paton","last_name":"Winscum","address":"52 Elmside Plaza","state":"FL","city":"Orlando","zip":"20800","email":"pwinscum1@google.cn","phone":"407-432-9102"},
		{"id":3,"first_name":"Cesare","last_name":"Britland","address":"95116 Sunnyside Pass","state":"UT","city":"Salt Lake City","zip":"62459","email":"cbritland2@nydailynews.com","phone":"801-213-5756"},
		{"id":4,"first_name":"Georgi","last_name":"Nestor","address":"2469 Iowa Junction","state":"CA","city":"Salinas","zip":"88901","email":"gnestor3@cnbc.com","phone":"831-898-2602"},
		{"id":5,"first_name":"Lezley","last_name":"Juckes","address":"7 Barnett Park","state":"TX","city":"College Station","zip":"28411","email":"ljuckes4@webs.com","phone":"979-140-7390"},
		{"id":6,"first_name":"Leupold","last_name":"Sertin","address":"85987 Crownhardt Hill","state":"KY","city":"Lexington","zip":"79341","email":"lsertin5@dagondesign.com","phone":"859-405-7783"},
		{"id":7,"first_name":"Gwenni","last_name":"Nollet","address":"8758 Bultman Place","state":"PA","city":"Pittsburgh","zip":"92927","email":"gnollet6@mac.com","phone":"412-156-3263"},
		{"id":8,"first_name":"Lucy","last_name":"Robyns","address":"2 Morningstar Point","state":"CA","city":"San Francisco","zip":"70970","email":"lrobyns7@facebook.com","phone":"415-299-3554"},
		{"id":9,"first_name":"Artemis","last_name":"Calverley","address":"4610 Sunnyside Drive","state":"TX","city":"San Antonio","zip":"03713","email":"acalverley8@drupal.org","phone":"210-480-9598"},
		{"id":10,"first_name":"Denny","last_name":"Shallow","address":"467 Corry Trail","state":"GA","city":"Atlanta","zip":"75985","email":"dshallow9@ask.com","phone":"770-486-0256"},
		{"id":11,"first_name":"Aile","last_name":"Simka","address":"037 Cambridge Center","state":"OH","city":"Springfield","zip":"10919","email":"asimkaa@blogspot.com","phone":"937-924-1914"},
		{"id":12,"first_name":"Davy","last_name":"Olman","address":"834 Summer Ridge Pass","state":"NY","city":"Rochester","zip":"49111","email":"dolmanb@wordpress.com","phone":"585-858-6322"},
		{"id":13,"first_name":"Katine","last_name":"Hunton","address":"3 Surrey Plaza","state":"DC","city":"Washington","zip":"35087","email":"khuntonc@rakuten.co.jp","phone":"202-205-8980"},
		{"id":14,"first_name":"Zarah","last_name":"Luquet","address":"1678 Mallory Lane","state":"CO","city":"Boulder","zip":"22523","email":"zluquetd@naver.com","phone":"303-771-9364"},
		{"id":15,"first_name":"Teodor","last_name":"Nevins","address":"5736 3rd Park","state":"MI","city":"Midland","zip":"07805","email":"tnevinse@linkedin.com","phone":"989-755-8533"}];

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
		message = 'Add Contact';
		contacts = contacts;
		showModal = true;
	};

	const editContactButton = (id) => {
		editableContact = contacts.find(contact => contact.id === id);
		console.log(editableContact);
		message = 'Edit Contact';
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
	<h2>{message}</h2>

	{#if message === 'Add Contact'}
		<AddForm {count} on:addContact={addContact}>
		</AddForm>
	{:else if message === 'Edit Contact'}
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
		color: #96badd;
	}
	table{
		overflow-y: scroll;
		height: 500px;
		display: inline-block;
		margin-bottom: 36px;
	}
	th {
		color: #39343a;
		text-transform: uppercase;
		cursor: pointer;
	}
	th, td, tr {
		font-family: 'Montserrat', sans-serif;
		text-align: left;
		padding: 15px;
	}
	td{
		color: #666667
	}
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
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
	h2{
		color: #121212;
		font-family: 'Montserrat', sans-serif;
		font-size: 30px;
	}
	input[type=text] {
		font-family: 'Montserrat', sans-serif;
        margin: 5px 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 6px;
        box-sizing: border-box;
    }
	h1 {
		color: #121212;
		font-family: 'Montserrat', sans-serif;
		font-size: 48px;
		font-weight: bold;
	}
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>