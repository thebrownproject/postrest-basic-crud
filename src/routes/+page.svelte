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
	const fetchData = () => {
		fetch('https://egzkoqyyrrtkijcmyowa.supabase.co/rest/v1/region', {
			method: 'GET',
			headers: { apiKey: apiKey }
		})
			.then((response) => response.json())
			.then((data) => {
				tableData.set(data);
				console.log($tableData);
			});
	};

	onMount(() => {
		fetchData();
	});
</script>

<h1>Welcome to SvelteKit!</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>
