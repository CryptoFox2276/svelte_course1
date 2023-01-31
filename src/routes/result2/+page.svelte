<script>
	import { memory } from '../stores.js';
	import { onMount } from 'svelte';
	import { fly, fade } from 'svelte/transition';

	let columns = [
		{
			header: 'First Name',
			accessor: 'firstName'
		},
		{
			header: 'Last Name',
			accessor: 'lastName'
		},
		{
			header: 'Car Brand',
			accessor: 'carBrand'
		},
		{
			header: 'Zip Code',
			accessor: 'zipCode'
		},
		{
			header: 'Car Model',
			accessor: 'carModel'
		},
		{
			header: 'First Registration',
			accessor: 'firstRegistration'
		}
	];

	let tableData = [];

	memory.subscribe(value => {
		tableData = value;
	});

	onMount(() => {
		console.log(memory);
		columns = columns.map((c) => {
			return c;
		});
	});
</script>

<svelte:head>
	<title>Result-2</title>
	<meta name="description" content="Result 2" />
</svelte:head>

<section>
	<div class="container">
		<div class="row text-center">
			<h2 transition:fade>Result-2</h2>
			<p transition:fade>You can see all information about full name and car</p>
		</div>
		<hr />
		<br />
		<div class="container">
			<div class="w-100">
				<div class="pb-2">
					<span transition:fade>Total: {tableData.length}</span>
				</div>
				<table>
					<thead>
						<tr transition:fade>
							<td>No</td>
							<td>FullName</td>
							<td>Car</td>
						</tr>
					</thead>

					{#if tableData.length > 0}
						{#each tableData as row, i}
							<tr transition:fade>
								<td>{i + 1}</td>
								<td>
									{row['firstName'] + ' ' + row['lastName']}
								</td>
								<td>
									{row['carModel'] + ' ' + row['carBrand']}
								</td>
							</tr>
						{/each}
					{:else}
						<tr transition:fade><td>No data</td></tr>
					{/if}
				</table>
			</div>
		</div>
	</div>
</section>

<style>
	tr > td {
		width: 45%;
	}
	td:first-child {
		width: 10%;
	}
</style>
