<script>
	import { toolData } from '$lib/player/ToolData.svelte.js';

	import ToolbarWheel from '$lib/components/toolbar/ToolbarWheel.svelte';
	import { preventDefault } from 'svelte/legacy';

	const toolBarMax = 10;

	let activeToolIndex = $state(1);
	$effect(() => {
		if (currentToolData) activeToolIndex = null;
	});

	let activeToolbarIndex = $state(1);

	let currentToolData = $derived(toolData[activeToolbarIndex - 1]);

	let availableTools = $derived(currentToolData?.filter((toolData) => toolData.tool));
</script>

<div class="frame">
	<ToolbarWheel {activeToolbarIndex} />
	<div class="toolbar-items">
		{#each currentToolData as toolItem}
			<div class="toolbar-item" class:active={activeToolIndex === toolItem.index}>
				{#if toolItem.tool}
					<div class="label">{toolItem.label}</div>
				{/if}
				<div class="icon" class:filled={toolItem.tool}>
					{#if toolItem.tool}
						<img class="icon-image" src={toolItem.tool} alt={toolItem.label} />
					{/if}
				</div>
			</div>
		{/each}
	</div>
</div>

<svelte:window
	on:keydown={(e) => {
		const key = e.key;
		if (/^[0-9]$/.test(key)) {
			// Map '1' to 0, '2' to 1, ..., '9' to 8, '0' to 9
			const index = key === '0' ? 9 : parseInt(key) - 1;
			if (index < availableTools.length) activeToolIndex = index;
		}
	}}
	on:wheel={(e) => {
		switch (true) {
			case e.altKey:
				// Change active toolbar
				if (e.deltaY > 0) {
					activeToolbarIndex = ((activeToolbarIndex - 2 + toolBarMax) % toolBarMax) + 1;
				} else {
					activeToolbarIndex = (activeToolbarIndex % toolBarMax) + 1;
				}
				break;
			case e.ctrlKey:
				// Save this for zooming
				break;
			case e.shiftKey:
				break;
			case !e.altKey && !e.ctrlKey && !e.shiftKey:
				// Change active tool
				if (e.deltaY > 0) {
					activeToolIndex = (activeToolIndex - 1 + availableTools.length) % availableTools.length;
				} else {
					activeToolIndex = (activeToolIndex + 1) % availableTools.length;
				}
				break;
		}
	}}
/>

<style>
	:root {
		--black-fade: rgba(0, 0, 0, 0.5);
		--white-fade: rgba(255, 255, 255, 0.4);
		--white-less: rgba(255, 255, 255, 0.2);
		--orange: rgb(228, 147, 68);
		--orange-fade: rgb(228, 147, 68, 0.5);
	}

	.frame {
		/* border: solid red 1px; */

		position: absolute;
		left: 50%;
		bottom: 30px;
		translate: -50% 0%;

		display: grid;
		grid-template-columns: 1fr auto 1fr;
		place-items: end;
		place-content: center;
		gap: 3px;
	}

	.toolbar-items {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: end;
		gap: 10px;
	}

	.toolbar-item {
		transition: translate 0.1s ease-out;
	}

	.toolbar-item.active {
		translate: 0px -10px;
	}

	.toolbar-item.active .label {
		background-color: var(--orange);
	}

	.toolbar-item.active .icon {
		background-color: var(--orange-fade);
	}

	.label {
		width: 72px;
		height: 20px;
		background-color: var(--black-fade);
		color: white;
		font-size: 15pt;
		font-weight: 700;

		text-align: center;
		line-height: 15pt;
	}

	.icon {
		width: 72px;
		height: 72px;
		background-color: var(--white-fade);
		padding: 4px;
		&:not(.filled) {
			background-color: var(--white-less);
		}

		display: flex;
	}
</style>
