<!-- Import Montserrat directly font from Google -->
<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700&display=swap" rel="stylesheet">
</svelte:head>

<script>
	// Component Imports
	import Modal from './Modal.svelte';
	import AddForm from './AddForm.svelte';
	import EditForm from './EditForm.svelte';

	// Contact List
	let contacts = [{"id":1,"first_name":"Gerry","last_name":"Wiggins","address":"758 Corry Street","state":"PA","city":"Philadelphia","zip":"50675","email":"gwiggins0@salon.com","phone":"215-985-3520"},
					{"id":2,"first_name":"Zea","last_name":"Ellingsworth","address":"589 Hovde Parkway","state":"FL","city":"Miami","zip":"73338","email":"zellingsworth1@blogs.com","phone":"786-804-7902"},
					{"id":3,"first_name":"Garey","last_name":"Keilloh","address":"1 Drewry Lane","state":"WV","city":"Huntington","zip":"34049","email":"gkeilloh2@360.cn","phone":"304-238-9892"},
					{"id":4,"first_name":"Herc","last_name":"O'Flannery","address":"0693 Ramsey Point","state":"TN","city":"Memphis","zip":"81666","email":"hoflannery3@1und1.de","phone":"901-448-4789"},
					{"id":5,"first_name":"Raffaello","last_name":"Ivimy","address":"894 Melrose Trail","state":"MA","city":"Newton","zip":"92245","email":"rivimy4@nifty.com","phone":"508-949-8881"},
					{"id":6,"first_name":"Charissa","last_name":"Innett","address":"48 Oneill Place","state":"UT","city":"Provo","zip":"62451","email":"cinnett5@addtoany.com","phone":"801-281-5128"},
					{"id":7,"first_name":"Aviva","last_name":"Dysert","address":"51 Schlimgen Crossing","state":"PA","city":"Philadelphia","zip":"62658","email":"adysert6@businessweek.com","phone":"267-956-2013"},
					{"id":8,"first_name":"Sheila-kathryn","last_name":"Hunnisett","address":"88221 7th Place","state":"CA","city":"Santa Barbara","zip":"22695","email":"shunnisett7@delicious.com","phone":"805-668-9396"},
					{"id":9,"first_name":"Kimbra","last_name":"Samet","address":"2 Morningstar Park","state":"PA","city":"Johnstown","zip":"87554","email":"ksamet8@imageshack.us","phone":"814-606-6461"},
					{"id":10,"first_name":"Delmer","last_name":"Sleep","address":"79 Hermina Court","state":"VA","city":"Portsmouth","zip":"11909","email":"dsleep9@google.com.br","phone":"757-656-4360"},
					{"id":11,"first_name":"Inger","last_name":"Klaggeman","address":"6 Northland Alley","state":"PA","city":"Philadelphia","zip":"07820","email":"iklaggemana@angelfire.com","phone":"215-743-0568"},
					{"id":12,"first_name":"Phaedra","last_name":"Sammes","address":"1 Helena Terrace","state":"CO","city":"Colorado Springs","zip":"08789","email":"psammesb@tripadvisor.com","phone":"719-748-1791"},
					{"id":13,"first_name":"Rodrick","last_name":"Ridett","address":"93 Green Circle","state":"TX","city":"Austin","zip":"73907","email":"rridettc@usatoday.com","phone":"512-774-5940"},
					{"id":14,"first_name":"Polly","last_name":"Ikringill","address":"2 Stuart Junction","state":"OK","city":"Oklahoma City","zip":"86138","email":"pikringilld@bing.com","phone":"405-564-7627"},
					{"id":15,"first_name":"Fenelia","last_name":"Bedberry","address":"181 Acker Street","state":"OH","city":"Dayton","zip":"80907","email":"fbedberrye@wix.com","phone":"937-740-4617"}];
	
	let message = 'default';	// Message used to call correct functions so that our Modal can be multi-functional
	let showModal = false;		// Boolean for whether our Modal is showing
	$: count = Math.max.apply(Math, contacts.map(function(o) { return o.id; })) + 1;	// Reactive count variable so that our IDs are always unique and 1 higher than current max.
	let editableContact = -1;	// Variable used as a parameter for our EditForm component and EditContactButton function

	let prefix = '';	// Filter prefix
	/* 
	sortedData is our reactive list variable that is displayed as the table on our CRUD SPA. It is continuously updated based on
	our current contacts list as well as the current prefix set in the search filter.
	**NOTE** we use this for 2 reasons, one is to ensure the protection of our contact data, and two, svelte requires updated variables
	to be reassigned in many cases.
	*/
	$: sortedData = prefix ? contacts.filter(contact => {
		return (contact.id === parseInt(prefix)) ||
		(contact.first_name.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.last_name.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.address.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.state.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.city.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.zip.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.email.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase())) ||
		(contact.phone.toLocaleLowerCase().startsWith(prefix.toLocaleLowerCase()));
	}) : contacts;

	let highlight = "id";	// Our highlight variable is used to determine which column is currently selected for table sorting.
	const headers = Object.keys(contacts[0]); // Our headers list consists of the keys in our contacts data list.

	/*
	sortByString(header) function will take in the header as a string to determine which column we are sorting our table in
	ascending order. It also will activate our highlight variable (defaulted to "id").
	*/
	const sortByString = (header) => {
		sortedData = sortedData.sort((obj1, obj2) => {
			if(obj1[header] < obj2[header]){
				return -1;
			}
			else if (obj1[header] > obj2[header]){
				return 1;
			}
			return 0;
		});
		highlight = header;
	};

	// Simple function to close our Modal and set message to default.
	const closeModal = () => {
		message = 'default';
		showModal = false;
	};

	// Given a row "id", deleteContactButton will delete the contact from the table and our contact list.
	const deleteContactButton = (id) => {
		contacts = contacts.filter((contact) => contact.id != id);
	};

	// addContactButton will set our Modal to AddForm and pull up the Modal
	const addContactButton = () => {
		message = 'Add Contact';
		showModal = true;
	};

	/*
	Given a row "id", editContactButton will set our editableContact to the matching contact in our list,
	set our Modal to Edit Contact, and pull up our Modal
	*/
	const editContactButton = (id) => {
		editableContact = contacts.find(contact => contact.id === id);
		message = 'Edit Contact';
		showModal = true;
	};

	/*
	addContact will take in our event data (e) that contains the new contact information to be added to our contact list,
	reassign our contact list to contact + contacts and close our Modal.
	*/
	const addContact = (e) => {
		const contact = e.detail;
		contacts = [contact, ...contacts];
		closeModal();
	};

	/*
	editContact takes in our event data (e) that contains the contact to be updated in our contacts list, map the
	edited contacts "id" to the contacts list "id" and set the data accordingly. Reassigns contacts, closes Modal.
	*/
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

<!-- 
	Our Modal component is essentially our form that is used to both edit and add contacts. It is activated
	based on our showModal boolean, sets the message of the form (edit or add) and calls the correct form.
	The on:click event is called when the user clicks outside of the form, in the case that they need to
	exit the modal. on:addContact and on:editContact are dispatch events sent from the respective AddForm
	and EditForm components containing the contact data to be used in our addContact and editContact functions
	above.
 -->
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

<!-- 
	Our main consists of our header, table, add contact button and search filter.
 -->
<main>
	<h1>Contacts</h1>
	<center>
		<table id="contacts-table">

			<!-- 
				Our columns are created here based on the keys in our contacts list. We loop through and set each column, 
				set the highlight (defaulted to "id") and create our on:click event sending the header string to our
				sort function. The replace is there to make our columns more readable for the user.
			 -->
			<tr>
				{#each headers as header}
					<th class:highlighted={header === highlight} on:click={() => sortByString(header)}>{header.replace('_', ' ')}</th>
				{/each}
			</tr>
			
			<!-- Create table data and edit/delete buttons -->
			{#each sortedData as contact}
				<tr>
					<td>{contact.id}</td>
					<td>{contact.first_name}</td>
					<td>{contact.last_name}</td>
					<td>{contact.address}</td>
					<td>{contact.state}</td>
					<td>{contact.city}</td>
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

	<!-- Binds our prefix to the value of our search filter for instantaneous filtering -->
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
		font-size: 60px;
		font-weight: bold;
	}
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>