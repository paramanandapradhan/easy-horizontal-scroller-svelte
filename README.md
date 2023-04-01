# easy-horizontal-scroller-svelte

A lightweight and user-friendly library for implementing horizontal scrolling on both web and mobile UIs. This intuitive scroller relies solely on the drag feature, providing a seamless and responsive experience across various devices.

# Install

```sh
npm install @cloudparker/easy-horizontal-scroller-svelte --save-dev
```

# Usage

```js
<script>
	import EasyHorizontalScroller from '@cloudparker/easy-horizontal-scroller-svelte';
</script>

<EasyHorizontalScroller height="136px">
	{#each Array(100) as _, index}
		<div class="item">{index}</div>
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
```

# Props
 