<script>
	import { onMount } from 'svelte';
	import { on } from 'svelte/events';
	import { writable } from 'svelte/store';

	// Create a writable store to hold the database table data
	const tableData = writable([]);

	class Region {
		constructor(name, targetBudget, actualSales) {
			this.name = name;
			this.targetBudget = targetBudget;
			this.actualSales = actualSales;
		}
	}

	const apiKey =
		'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImVnemtvcXl5cnJ0a2lqY215b3dhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDMyOTkyMTcsImV4cCI6MjA1ODg3NTIxN30.M19dgayNuzJfuI_UivY8CTlFcSSxPChgsa6DV_0sw4E';

	// Function to fetch data from the database with REST API
	const getData = () => {
		fetch('https://egzkoqyyrrtkijcmyowa.supabase.co/rest/v1/region', {
			method: 'GET',
			headers: { apiKey: apiKey }
		})
			.then((response) => response.json())
			.then((data) => {
				const regions = data.map(
					(item) => new Region(item.name, item.target_budget, item.actual_sales)
				);
				tableData.set(regions);
				console.log($tableData);
			})
			.catch((error) => console.error('Error fetching data:', error));
	};

	// Function to post data to the database with REST API
	const postData = (newRegion) => {
		fetch('https://egzkoqyyrrtkijcmyowa.supabase.co/rest/v1/region', {
			method: 'POST',
			headers: {
				apikey: apiKey,
				Authorization: `Bearer ${apiKey}`,
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({
				name: newRegion.name,
				target_budget: newRegion.target_budget,
				actual_sales: newRegion.actual_sales
			})
		});
		if (response.ok) {
			getData();
		} else {
			console.error('Error posting data:', response.statusText);
		}
	};

	onMount(() => {
		getData();
	});
</script>

<h1>Welcome to SvelteKit!</h1>
{#each $tableData as region}
	<div>
		<h2>{region.name}</h2>
		<p>Target Budget: {region.targetBudget}</p>
		<p>Actual Sales: {region.actualSales}</p>
	</div>
{/each}
