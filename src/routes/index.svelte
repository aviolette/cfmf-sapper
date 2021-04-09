

<svelte:head>
	<title>Chicago Farmers Market Finder</title>
</svelte:head>

<h1>Chicago Farmers' Market Finder</h1>

<script>
	import Box from '../components/Box.svelte';

	let today = new Date().toLocaleDateString("en-US")
	async function getData() {
		const response = await fetch("/foobar.json")
		return await response.json()
	}
	let marketPromise = getData()
</script>

<h2>Markets Open Today - {today}</h2>

{#await marketPromise}
{:then markets}
	{#each markets["data"] as market}
		<Box>
			{market["name"]}<br/>
			7am-1:30pm<br/>
			{market["location"]["name"]}<br/>

		</Box>
	{/each}
{/await}

