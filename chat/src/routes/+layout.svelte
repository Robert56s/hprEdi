<script>
	import favicon from '$lib/assets/favicon.svg';
  import { nonpassive } from 'svelte/legacy';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<style>
		@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100..900;1,100..900&display=swap');
		body {
			margin: 0;
			font-family: "Roboto", sans-serif;
			font-optical-sizing: auto;
			font-weight: <weight>;
			font-style: normal;
			font-variation-settings:
			"wdth" 100;
		}
	</style>
</svelte:head>

<div class="app">
	{@render children()}
</div>

<style>
	.app {
		height: 100vh;
		width: 100vw;
		display: flex;
		flex-direction: column;
	}
</style>