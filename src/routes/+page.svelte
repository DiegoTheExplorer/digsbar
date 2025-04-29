<script lang="ts">
	import { onMount } from 'svelte';
	import * as zebar from 'zebar';
	import type {
		BatteryOutput,
		CpuOutput,
		GlazeWmOutput,
		MemoryOutput,
		DateOutput,
		NetworkOutput,
		WeatherOutput
	} from 'zebar';

	let battery = $state<BatteryOutput | null>();
	let cpu = $state<CpuOutput | null>();
	let date = $state<DateOutput | null>();
	let glazewm = $state<GlazeWmOutput | null>();
	let memory = $state<MemoryOutput | null>();
	let network = $state<NetworkOutput | null>();
	let weather = $state<WeatherOutput | null>();

	onMount(() => {
		const providers = zebar.createProviderGroup({
			battery: { type: 'battery' },
			cpu: { type: 'cpu' },
			date: { type: 'date', formatting: 'HH:mm' },
			glazewm: { type: 'glazewm' },
			memory: { type: 'memory' },
			network: { type: 'network' },
			weather: { type: 'weather' }
		});

		providers.onOutput(() => {
			battery = providers.outputMap.battery;
			cpu = providers.outputMap.cpu;
			date = providers.outputMap.date;
			glazewm = providers.outputMap.glazewm;
			memory = providers.outputMap.memory;
			network = providers.outputMap.network;
			weather = providers.outputMap.weather;
		});
	});
</script>

<div class="zebar">
	<div class="group left-group">
		<h1>Welcome to SvelteKit</h1>
	</div>
	<div class="group middle-group">
		<i class="nf-oct-terminal"></i>
	</div>
	<div class="group right-group">
		<p>{memory?.usage}</p>
	</div>
</div>

<style>
	* {
		margin: 0;
		padding: 0;
	}
	.zebar {
		font-family: Hurmit;
		background-color: transparent;
		max-height: 40px;
		display: grid;
		grid-auto-columns: minmax(0, 1fr);
		grid-auto-flow: column;
	}

	.group {
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.left-group {
		background-color: red;
	}

	.middle-group {
		background-color: green;
	}

	.right-group {
		background-color: blue;
	}
</style>
