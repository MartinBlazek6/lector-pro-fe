<!--<svelte:head>-->
<!--	<title>Table</title>-->
<!--	<meta name="description" content="About this app"/>-->
<!--</svelte:head>-->
<style>
	table {
		border-collapse: collapse;
		width: 100%;
	}

	th, td {
		padding: 8px;
		text-align: left;
		border-bottom: 1px solid #ddd;
	}

	th {
		background-color: #f2f2f2;
		font-weight: bold;
	}

	tr:hover {
		background-color: #f5f5f5;
	}
</style>
<script>
	import {onMount} from "svelte";

	let lections = [];
	let errorMessage = '';
	// let server = 'http://localhost:8080';
	let server = 'https://lector-tool.up.railway.app';

	const fetchLections = async () => {
		try {
			const response = await fetch(server + '/api/v1/lections');
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
</script>

<div class="text-column">
	<h1>List of Lections</h1>

	{#if errorMessage}
		<p>{errorMessage}</p>
	{:else if lections.length > 0}
		<table>
			<thead>
			<tr>
				<th>ID</th>
				<th>Title</th>
				<th>Level</th>
				<th>Date</th>
			</tr>
			</thead>
			<tbody>
			{#each lections as lection}
				<tr>
					<td>{lection.lectionId}</td>
					<td>{lection.title}</td>
					<td>{lection.level}</td>
					<td>{new Date(lection.date).toLocaleDateString()}</td>
				</tr>
			{/each}
			</tbody>
		</table>
	{:else}
		<p>Loading lections...</p>
	{/if}

</div>

<!--<svelte:head>-->
<!--	<title>Admin</title>-->
<!--	<meta name="description" content="About this app" />-->
<!--</svelte:head>-->
<!--<script>-->

<!--	let errorMessage = '';-->
<!--	let title = '';-->
<!--	let level = '';-->
<!--	let date = '';-->
<!--	// let server = 'http://localhost:8080';-->
<!--	let server = 'https://lector-tool.up.railway.app';-->

<!--	const createLection = async () => {-->
<!--		try {-->
<!--			const response = await fetch(server+`/api/v1/createLection?title=${title}&level=${level}&date=${date}`, {-->
<!--				method: 'POST',-->
<!--				headers: {-->
<!--					'Content-Type': 'application/json'-->
<!--				}-->
<!--			});-->

<!--			if (response.ok) {-->
<!--				const newLection = await response.json();-->
<!--				// Do something with the new lection, such as add it to the list of lections-->
<!--				console.info('ok')-->
<!--			} else {-->
<!--				errorMessage = 'Failed to create lection.';-->
<!--			}-->
<!--		} catch (error) {-->
<!--			errorMessage = 'An error occurred while creating the lection.';-->
<!--		}-->
<!--	};-->
<!--</script>-->

<!--<style>-->
<!--	h2 {-->
<!--		font-size: 2rem;-->
<!--		margin-bottom: 1rem;-->
<!--		text-align: center;-->
<!--		text-transform: uppercase;-->
<!--	}-->

<!--	ul {-->
<!--		list-style: none;-->
<!--		margin: 0;-->
<!--		padding: 0;-->
<!--	}-->

<!--	li {-->
<!--		margin-bottom: 1rem;-->
<!--	}-->

<!--	label {-->
<!--		display: block;-->
<!--		font-size: 1.2rem;-->
<!--		font-weight: bold;-->
<!--		margin-bottom: 0.5rem;-->
<!--		text-transform: uppercase;-->
<!--	}-->

<!--	input[type="text"],-->
<!--	input[type="email"] {-->
<!--		border: 2px solid #ccc;-->
<!--		border-radius: 0.5rem;-->
<!--		font-size: 1.2rem;-->
<!--		padding: 0.5rem;-->
<!--		width: 100%;-->
<!--	}-->

<!--	button[type="submit"] {-->
<!--		background-color: #007bff;-->
<!--		border: none;-->
<!--		border-radius: 0.5rem;-->
<!--		color: #fff;-->
<!--		cursor: pointer;-->
<!--		font-size: 1.2rem;-->
<!--		padding: 0.5rem 1rem;-->
<!--		text-transform: uppercase;-->
<!--		transition: background-color 0.2s ease-in-out;-->
<!--	}-->

<!--	button[type="submit"]:hover {-->
<!--		background-color: #0062cc;-->
<!--	}-->

<!--	p {-->
<!--		font-size: 1.2rem;-->
<!--		margin-bottom: 1rem;-->
<!--		text-align: center;-->
<!--	}-->
<!--</style>-->

<!--<form on:submit={createLection}>-->
<!--	<label>-->
<!--		Title:-->
<!--		<input type="text" bind:value={title}>-->
<!--	</label>-->
<!--	<label>-->
<!--		Level:-->
<!--		<input type="text" bind:value={level}>-->
<!--	</label>-->
<!--	<label>-->
<!--		Date:-->
<!--		<input type="date" bind:value={date}>-->
<!--	</label>-->
<!--	<button type="submit">Create Lection</button>-->
<!--</form>-->
