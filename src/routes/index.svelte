

<svelte:head>
	<title>Chicago Farmers Market Finder</title>
</svelte:head>

<script context="module">
	function zeroFilled(n) { return ('00'+n).slice(-2); }

	export async function preload(page, session) {
		let fileName = ''
		if (page.path === '/') {
			const now = new Date()
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
	import Map from '../components/Map.svelte';
	import MapMarker from '../components/MapMarker.svelte';

	let today = new Date().toLocaleDateString("en-US")
	export let markets


</script>
<h2>Farmers' Markets Open Today - {today}</h2>

<Map lat={41.8336479} lon={-87.8720449} zoom={10}>

	{#each markets["data"] as market}
		<MapMarker lat="{market['location']['latitude']}" lon="{market['location']['longitude']}" label="<h2><a target='_blank' href='{market['url']}'>{market['name']}</a></h2>{market['location']['address']}<br/>Hours:<br/>{market['startTimeReadable']} - {market['endTimeReadable']}"/>
	{/each}
</Map>