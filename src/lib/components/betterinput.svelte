<script>
	export let value = '';
	export let placeholder = '';
	export let rows = 1;
	export let timeout = 100;
	let isTyping = false;

	$: placeholderValue = value.length ? '' : placeholder;

	function slowType(e) {
		if(timeout == 0) {
			return;
		} 
		e.preventDefault();
		if (isTyping) return false;

		let isShortcut = e.ctrlKey || e.metaKey || e.altKey;
		if (!isShortcut && /^.$/i.test(e.key)) {
			console.log(e);
			isTyping = true;
			setTimeout(() => {
				value = value + e.key;
				isTyping = false;
			}, timeout);
		}
	}
</script>

<div class="a">
	<span>{placeholderValue}</span>
	{#if rows == 1}
		<div
			contenteditable="true"
			bind:textContent={value}
			on:keypress={(e) => slowType(e)}
			on:change|preventDefault
			on:input|preventDefault
			on:keyup|preventDefault
		/>
	{:else}
		<textarea
			{rows}
			bind:value
			on:keypress={(e) => slowType(e)}
			on:change|preventDefault
			on:input|preventDefault
			on:keyup|preventDefault
		/>
	{/if}
</div>

<style lang="postcss">
	div {
		@apply relative;
	}
	span {
		@apply absolute text-sm inset-3 text-gray-500 pointer-events-none z-10;
	}
	[contenteditable] {
		@apply p-2 border border-black bg-white;
	}

	textarea {
		@apply w-full;
	}
</style>
