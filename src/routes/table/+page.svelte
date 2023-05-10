<!--<svelte:head>-->
<!--	<title>Table</title>-->
<!--	<meta name="description" content="About this app"/>-->
<!--	<style>-->
<!--		table {-->
<!--			border-collapse: collapse;-->
<!--			width: 100%;-->
<!--		}-->

<!--		th, td {-->
<!--			padding: 8px;-->
<!--			text-align: left;-->
<!--			border-bottom: 1px solid #ddd;-->
<!--		}-->

<!--		th {-->
<!--			background-color: #f2f2f2;-->
<!--			font-weight: bold;-->
<!--		}-->

<!--		tr:hover {-->
<!--			background-color: #f5f5f5;-->
<!--		}-->
<!--	</style>-->
<!--</svelte:head>-->

<!--<script>-->
<!--	import {onMount} from "svelte";-->

<!--	let lections = [];-->
<!--	let errorMessage = '';-->
<!--	// let server = 'http://localhost:8080';-->
<!--	let server = 'https://lector-tool.up.railway.app';-->

<!--	const fetchLections = async () => {-->
<!--		try {-->
<!--			const response = await fetch(server + '/api/v1/lections');-->
<!--			if (response.ok) {-->
<!--				lections = await response.json();-->
<!--			} else {-->
<!--				errorMessage = 'Failed to fetch lections.';-->
<!--			}-->
<!--		} catch (error) {-->
<!--			errorMessage = 'An error occurred while fetching lections.';-->
<!--		}-->
<!--	};-->


<!--	onMount(fetchLections);-->
<!--</script>-->

<!--&lt;!&ndash;<div className="text-column">&ndash;&gt;-->
<!--&lt;!&ndash;	<h1>List of Lections</h1>&ndash;&gt;-->

<!--&lt;!&ndash;	{#if errorMessage}&ndash;&gt;-->
<!--&lt;!&ndash;		<p>{errorMessage}</p>&ndash;&gt;-->
<!--&lt;!&ndash;	{:else if lections.length > 0}&ndash;&gt;-->
<!--&lt;!&ndash;		<table>&ndash;&gt;-->
<!--&lt;!&ndash;			<thead>&ndash;&gt;-->
<!--&lt;!&ndash;			<tr>&ndash;&gt;-->
<!--&lt;!&ndash;				<th>ID</th>&ndash;&gt;-->
<!--&lt;!&ndash;				<th>Title</th>&ndash;&gt;-->
<!--&lt;!&ndash;				<th>Level</th>&ndash;&gt;-->
<!--&lt;!&ndash;				<th>Date</th>&ndash;&gt;-->
<!--&lt;!&ndash;			</tr>&ndash;&gt;-->
<!--&lt;!&ndash;			</thead>&ndash;&gt;-->
<!--&lt;!&ndash;			<tbody>&ndash;&gt;-->
<!--&lt;!&ndash;			{#each lections as lection}&ndash;&gt;-->
<!--&lt;!&ndash;				<tr>&ndash;&gt;-->
<!--&lt;!&ndash;					<td>{lection.lectionId}</td>&ndash;&gt;-->
<!--&lt;!&ndash;					<td>{lection.title}</td>&ndash;&gt;-->
<!--&lt;!&ndash;					<td>{lection.level}</td>&ndash;&gt;-->
<!--&lt;!&ndash;					<td>{new Date(lection.date).toLocaleDateString()}</td>&ndash;&gt;-->
<!--&lt;!&ndash;				</tr>&ndash;&gt;-->
<!--&lt;!&ndash;			{/each}&ndash;&gt;-->
<!--&lt;!&ndash;			</tbody>&ndash;&gt;-->
<!--&lt;!&ndash;		</table>&ndash;&gt;-->
<!--&lt;!&ndash;	{:else}&ndash;&gt;-->
<!--&lt;!&ndash;		<p>Loading lections...</p>&ndash;&gt;-->
<!--&lt;!&ndash;	{/if}&ndash;&gt;-->

<!--&lt;!&ndash;</div>&ndash;&gt;-->
<!--{#if errorMessage}-->
<!--	<p>{errorMessage}</p>-->
<!--{:else if lections.length > 0}-->
<!--	<ul>-->
<!--		{#each lections as lection}-->
<!--			<li>lectionId: {lection.lectionId} -  {lection.title} - {lection.level} - {new Date(lection.date).toLocaleDateString()}</li>-->
<!--		{/each}-->
<!--	</ul>-->
<!--{:else}-->
<!--	<p>Loading lections...</p>-->
<!--{/if}-->
<script>
	import {onMount} from 'svelte';

	let lections = [];
	let errorMessage = '';
	let studentIdVar;
	let lectionIdVar;

	let firstName = '';
	let lastName = '';
	let email = '';
	// let server = 'http://localhost:8080';
	let server = 'https://lector-tool.up.railway.app';

	const fetchLections = async () => {
		try {
			const response = await fetch(server+'/api/v1/lections');
			if (response.ok) {
				lections = await response.json();
			} else {
				errorMessage = 'Failed to fetch lections.';
			}
		} catch (error) {
			errorMessage = 'An error occurred while fetching lections.';
		}
	};

	onMount(fetchLections);

	const addStudent = async () => {
		try {
			const response = await fetch(server+'/api/v1/addStudent', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					firstName,
					lastName,
					email
				})
			});

			if (response.ok) {
				const newStudent = await response.json();
				// Do something with the new student, such as add it to the list of students
			} else {
				errorMessage = 'Failed to add student.';
			}
		} catch (error) {
			errorMessage = 'An error occurred while adding the student.';
		}
	};

	const addStudentToLection = async () => {
		try {
			const response = await fetch(server+`/api/v1/addStudentToLection?lectionId=${lectionIdVar}&studentId=${studentIdVar}`, {
				method: 'POST'
			});
			if (response.ok) {
				// Refresh the lections list
				fetchLections();
			} else {
				errorMessage = 'Failed to add student to lection.';
			}
		} catch (error) {
			errorMessage = 'An error occurred while adding student to lection.';
		}
	};

</script>


<h2>All Lections:</h2>

<style>
	h2 {
		font-size: 2rem;
		margin-bottom: 1rem;
		text-align: center;
		text-transform: uppercase;
	}

	ul {
		list-style: none;
		margin: 0;
		padding: 0;
	}

	li {
		margin-bottom: 1rem;
	}

	label {
		display: block;
		font-size: 1.2rem;
		font-weight: bold;
		margin-bottom: 0.5rem;
		text-transform: uppercase;
	}

	input[type="text"],
	input[type="email"] {
		border: 2px solid #ccc;
		border-radius: 0.5rem;
		font-size: 1.2rem;
		padding: 0.5rem;
		width: 100%;
	}

	button[type="submit"] {
		background-color: #007bff;
		border: none;
		border-radius: 0.5rem;
		color: #fff;
		cursor: pointer;
		font-size: 1.2rem;
		padding: 0.5rem 1rem;
		text-transform: uppercase;
		transition: background-color 0.2s ease-in-out;
	}

	button[type="submit"]:hover {
		background-color: #0062cc;
	}

	p {
		font-size: 1.2rem;
		margin-bottom: 1rem;
		text-align: center;
	}
</style>


{#if errorMessage}
	<p>{errorMessage}</p>
{:else if lections.length > 0}
	<ul>
		{#each lections as lection}
			<li>lectionId: {lection.lectionId} -  {lection.title} - {lection.level} - {new Date(lection.date).toLocaleDateString()}</li>
		{/each}
	</ul>
{:else}
	<p>Loading lections...</p>
{/if}

<h2>Add Student:</h2>

<form on:submit|preventDefault={addStudent}>
	<div>
		<label for="firstName">First Name:</label>
		<input type="text" id="firstName" bind:value={firstName}/>
	</div>

	<div>
		<label for="lastName">Last Name:</label>
		<input type="text" id="lastName" bind:value={lastName}/>
	</div>

	<div>
		<label for="email">Email:</label>
		<input type="email" id="email" bind:value={email}/>
	</div>

	<button type="submit">Add Student</button>
</form>

<form on:submit|preventDefault={addStudentToLection}>
	<div>
		<label for="firstName">StudentId:</label>
		<input type="text" id="studentId" bind:value={studentIdVar}/>
	</div>

	<div>
		<label for="lastName">LectionId:</label>
		<input type="text" id="lectionId" bind:value={lectionIdVar}/>
	</div>


	<button type="submit">Add Student</button>
</form>







