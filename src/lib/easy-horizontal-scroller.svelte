<script lang="ts">
	import { cubicOut } from 'svelte/easing';
	import { onMount } from 'svelte';
	import { tweened } from 'svelte/motion';

	export let height: string;

	let scrollLeft = tweened(0, {
		duration: 800,
		easing: cubicOut
	});

	let startX = 0;
	let lastX = 0;
	let velocity = 0;
	let scrollContainerRef: HTMLDivElement;
	let scrollerRef: HTMLDivElement;
	let scrollerLeft: number = 0;
	let scrollerWidth: number;
	let isDragging: boolean = false;

	onMount(() => {
		scrollLeft.set(0);
	});

	function getEventXY(event: any): { x: number; y: number } {
		let x: number = 0;
		let y: number = 0;
		if (event) {
			if (event.touches && event.touches[0]) {
				x = event.touches[0].clientX;
				y = event.touches[0].clientY;
			} else {
				x = event.clientX;
				y = event.clientY;
			}
		}
		return { x, y };
	}

	function dragStart(e: any) {
		isDragging = true;
		let { x } = getEventXY(e);

		startX = x;
		lastX = x;
		velocity = 0;
		scrollerWidth = scrollerRef.scrollWidth - scrollerRef.offsetWidth;
	}

	function dragEnd() {
		isDragging = false;
		velocity = velocity * (Math.abs(velocity) / 5);
		updateScroll();
	}

	function drag(e: any) {
		if (!isDragging) return;
		let { x } = getEventXY(e);
		velocity = x - lastX;
		lastX = x;
		updateScroll();
	}

	function updateScroll() {
		if (!isDragging) return;
		requestAnimationFrame(() => {
			scrollLeft.update((value) => {
				scrollerLeft = value - velocity;
				if (scrollerLeft <= 0) scrollerLeft = 0;
				if (scrollerLeft >= scrollerWidth) scrollerLeft = scrollerWidth;
				return scrollerLeft;
			});
		});
	}

	onMount(() => {
		const touchOptions = {
			passive: true,
			capture: false
		};

		scrollContainerRef.addEventListener('touchstart', dragStart, touchOptions);
		scrollContainerRef.addEventListener('touchmove', drag, touchOptions);
		scrollContainerRef.addEventListener('touchend', dragEnd, touchOptions);
		scrollContainerRef.addEventListener('touchcancel', dragEnd, touchOptions);
	});
</script>

<svelte:window
	on:mouseup={dragEnd}
	on:mouseleave={dragEnd}
	on:touchend={dragEnd}
	on:touchcancel={dragEnd}
/>
<div
	class="hscroll-container"
	style:height
	bind:this={scrollContainerRef}
	on:mousedown={dragStart}
	on:mouseup|stopPropagation={dragEnd}
	on:mouseleave|stopPropagation={dragEnd}
	on:mousemove|stopPropagation={drag}
	on:touchstart|stopPropagation={dragStart}
	on:touchmove|stopPropagation={drag}
	on:touchend|stopPropagation={dragEnd}
	on:touchcancel|stopPropagation={dragEnd}
>
	<div class="scroller" bind:this={scrollerRef} style="transform: translateX({-$scrollLeft}px)">
		<slot />
	</div>
</div>

<style>
	.hscroll-container {
		display: flex;
		overflow: hidden;
		width: 100%;
		height: 100px;
		position: relative;
		scroll-behavior: smooth;
		user-select: none;
		touch-action: pan-y;
		will-change: transform;
	}

	.scroller {
		display: flex;
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		will-change: transform;
	}
</style>
