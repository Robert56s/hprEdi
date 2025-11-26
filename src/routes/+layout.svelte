<script>
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<style>
		* {
			margin: 0;
			padding: 0;
			box-sizing: border-box;
		}
	</style>
</svelte:head>

{@render children()}
