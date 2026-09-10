<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import inactiveFavicon from '$lib/assets/inactive.svg';
	import { onMount } from 'svelte';

	let { children } = $props();

	// Favicon visibility state
	let timeout: ReturnType<typeof setTimeout>;

	function updateFavicon() {
		clearTimeout(timeout);

		if (document.visibilityState === 'visible') {
			document.querySelector<HTMLLinkElement>('link[rel="icon"]')!.href = favicon;
		} else {
			timeout = setTimeout(() => {
				if (document.visibilityState === 'hidden') {
					document.querySelector<HTMLLinkElement>('link[rel="icon"]')!.href = inactiveFavicon;
				}
			}, 500);
		}
	}

	onMount(() => {
		updateFavicon();
		document.addEventListener('visibilitychange', updateFavicon);

		return () => {
			clearTimeout(timeout);
			document.removeEventListener('visibilitychange', updateFavicon);
		};
	});
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>linkपेड़</title>
</svelte:head>

{@render children()}