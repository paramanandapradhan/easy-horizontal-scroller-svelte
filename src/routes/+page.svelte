<script>
	import EasyHorizontalScroller from '$lib/easy-horizontal-scroller.svelte';
</script>

<EasyHorizontalScroller height="136px">
	{#each Array(500) as _, index}
		<div class="item" on:click={()=> console.log("clicked")}>{index}</div>
	{/each}
</EasyHorizontalScroller>

<style>
	.item {
		background-color: aquamarine;
		min-width: 128px;
		min-height: 128px;
		display: flex;
		align-items: center;
		justify-content: center;
		margin: 4px;
		border: 1px solid #d1d1d1;
		border-radius: 5px;
	}
</style>
