<script>
	import { createEventDispatcher } from 'svelte';

	let a = Math.floor(Math.random() * 9) + 1;
	let b = Math.floor(Math.random() * 9) + 1;
	let answer = a * b;

	let selected = 0;

	let error = false;

	$: console.log(selected);

	const dispatch = createEventDispatcher();

	function validate() {
		if (answer != selected) {
			error = true;
			return false;
		}

		dispatch('success');

		return true;
	}
</script>

<div class="grid grid-cols-1 gap-6 text-center" style="--IvgEsTpy9rl:{b}">
	<h1>Captcha</h1>
	<div class:error class="font-bold" style="--IvgEsTpy9rx:{a}">
		<span></span><span></span><span></span>
	</div>

	<div class="grid grid-cols-10 gap-2">
		{#each Array(100) as _, i}
			{#if selected == i}
				<span on:click={(e) => (selected = e.target.innerText)} class="selected">{i}</span>
			{:else}
				<span on:click={(e) => (selected = e.target.innerText)}>{i}</span>
			{/if}
		{/each}
	</div>
	<button on:click={validate}>Finish</button>
</div>

<style lang="postcss">
	.error {
		@apply text-red-600;
	}

	span {
		@apply border p-1 border-transparent cursor-pointer select-none;
	}

	.selected {
		@apply border border-gray-600 font-bold;
	}

	.font-bold span:first-child::before {
		counter-reset: IvgEsTpy9rx var(--IvgEsTpy9rx);
    	content: counter(IvgEsTpy9rx);
	}

	.font-bold span:last-child::before {
		counter-reset: IvgEsTpy9rl var(--IvgEsTpy9rl);
    	content: counter(IvgEsTpy9rl);
	}

	.font-bold span:nth-child(2)::before {
    	content: " * ";
	}
</style>
