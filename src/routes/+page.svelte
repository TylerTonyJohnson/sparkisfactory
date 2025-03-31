<script context="module">
	import Hud from '$lib/components/Hud.svelte';
	import Toolbar from '$lib/components/toolbar/Toolbar.svelte';

	import '../app.css';

	let screenElement;

	function lockAndLoad() {
		screenElement.requestFullscreen().then(() => {
			screenElement.requestPointerLock();
		});
	}
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
	<link
		href="https://fonts.googleapis.com/css2?family=Open+Sans:ital,wdth,wght@0,75..100,300..800;1,75..100,300..800&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="frame" bind:this={screenElement} onclick={lockAndLoad}>
	<Hud />
</div>

<style>
	.frame {
		height: 100vh;
		width: 100vw;
		background-image: url('/images/clean-background.jpg');
		background-size: cover;
		background-position: center;

		font-family: 'Open Sans', sans-serif;
		display: flex;
		place-items: center;
		place-content: center;
		overflow: hidden;

		/* border: solid red 1px; */
	}
</style>
