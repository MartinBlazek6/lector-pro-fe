<svelte:head>
	<title>Table</title>
	<meta name="description" content="About this app"/>
</svelte:head>

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
	import { onMount } from "svelte";
	import { afterUpdate } from "svelte";

	let lections = [];
	let errorMessage = '';
	// let server = 'http://localhost:8080';
	let server = 'https://lector-tool.up.railway.app';

	const fetchLections = async () => {
		try {
			const response = await fetch(`${server}/api/v1/lections`);
			if (response.ok) {
				lections = await response.json();
			} else {
				errorMessage = 'Failed to fetch lections.';
			}
		} catch (error) {
			errorMessage = 'An error occurred while fetching lections.';
		}
	};

	const sendInvoices = async (lectionId) => {
		try {
			const response = await fetch(`${server}/api/v1/sendInvoices?lectionId=${lectionId}`, {
				method: 'POST'
			});
			if (response.ok) {
				// Refresh the lections list
				fetchLections();
			} else {
				errorMessage = 'Failed to send invoice';
			}
		} catch (error) {
			errorMessage = 'An error occurred while sending the invoice';
		}
	};

	onMount(fetchLections);
	afterUpdate(() => {
		if (errorMessage) {
			setTimeout(() => {
				errorMessage = '';
			}, 3000);
		}
	});
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
				<th></th>
			</tr>
			</thead>
			<tbody>
			{#each lections as lection}
				<tr>
					<td>{lection.lectionId}</td>
					<td>{lection.title}</td>
					<td>{lection.level}</td>
					<td>{new Date(lection.date).toLocaleDateString()}</td>
					<td>
						<button type="submit" on:click={() => sendInvoices(lection.lectionId)}>Send invoice</button>
					</td>
				</tr>
			{/each}
			</tbody>
		</table>
	{:else}
		<p>Loading lections...</p>
	{/if}
</div>
