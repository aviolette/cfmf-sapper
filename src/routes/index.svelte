

<svelte:head>
	<title>Chicago Farmers Market Finder</title>
</svelte:head>

<h1>Chicago Farmers' Market Finder</h1>

<script context="module">
	function zeroFilled(n) { return ('00'+n).slice(-2); }

	export async function preload(page, session) {
		let fileName = ''
		if (page.path === '/') {
			const now = new Date(2021,5,4)
//			const now = new Date()
			fileName = now.getFullYear() + "-" + zeroFilled(now.getMonth()+1) + "-" + zeroFilled(now.getDate()) + ".json"
		}
		const response = await this.fetch("/dates/" + fileName).then(response => {
			if (!response.ok) {
				throw new Error('Unable to retrieve')
			}
			return response;
		}).catch(error => {
			return {
				json: function() {
					return {
						data: []
					}
				}
			}
		})
		const markets =  await response.json()
		return { markets }
	}
</script>

<script>
	import Box from '../components/Box.svelte';

	let today = new Date().toLocaleDateString("en-US")
	export let markets
</script>
<h2>Markets Open Today - {today}</h2>

{#each markets["data"] as market}
	<Box>
		{market["name"]}<br/>
		{market["startTime"]}-{market["endTime"]}<br/>
		{market["location"]["address"]}<br/>

	</Box>
{/each}

