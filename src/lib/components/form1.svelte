<script>
	import { createEventDispatcher } from 'svelte';
	import { subscribe } from 'svelte/internal';
	import BetterInput from './betterinput.svelte';

	let answer = {
		name: '',
		email: '',
		date: '',
		type: '0',
		details: '',
		subscribe: Math.random() < 0.5
	};

	let errors = [];

	let subscribePosition = Math.floor(Math.random() * 6);
	let optionsVisible = false;

	const dispatch = createEventDispatcher();

	function validate() {
		errors = [];

		if (answer.name.length < 2) {
			errors.push('Name to short');
		}
		if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(answer.email) == false) {
			errors.push('Invalid Email');
		}
		const timestamp = Date.parse(answer.date);
		if (isNaN(timestamp)) {
			errors.push('Invalid Date');
		} else if (new Date(timestamp) <= new Date(new Date().toDateString())) {
			errors.push('Date must be in the future');
		}
		if (answer.type == '0') {
			errors.push('Select a type');
		}
		if (answer.details.length <= 10) {
			errors.push('More details required');
		}
		if (!answer.subscribe) {
			errors.push('You must subscribe');
		}
		if (errors.length) {
			return false;
		}

		dispatch('success');

		return true;
	}

</script>

<div class="grid grid-cols-1 gap-6">
	<BetterInput placeholder="Full name" timeout={0} bind:value={answer.name} />

	<BetterInput placeholder="Email address" timeout={150} bind:value={answer.email} />
	<span class="block">
		<span>When is your event?</span>
		<input
			type="date"
			bind:value={answer.date}
			on:change|preventDefault|stopPropagation={() => false}
			on:input|preventDefault|stopPropagation={() => false}
			on:keydown|preventDefault|stopPropagation={() => false}
			on:keyup|preventDefault|stopPropagation={() => false}
			on:keypress|preventDefault|stopPropagation={() => false}
		/>
	</span>
	<select
		class="block"
		bind:value={answer.type}
		on:click|stopPropagation={() => {
			optionsVisible = true;
		}}
	>
		{#if optionsVisible}
			<option value="1">Corporate event</option>
			<option value="2">Wedding</option>
			<option value="3">Birthday</option>
			<option value="4">Other</option>
		{:else}
			<option disabled value="0">What type of event is it?</option>
		{/if}
	</select>
	<BetterInput placeholder="Additional details" timeout={100} bind:value={answer.details} rows={3} />
	<span class="c">
		{#each 'Subscribe to our random special offers'.split(' ') as word, i}
			<span class="d">{word} {' '}</span>
			{#if subscribePosition == i}
				{#if answer.subscribe}
					<span on:click={() => (answer.subscribe = false)}>✔️</span>
				{:else}
					<span on:click={() => (answer.subscribe = true)}>❌</span>
				{/if}
			{/if}
		{/each}
	</span>

	<div>
		{#each errors as error}
			<div class="text-red-400">{error}</div>
		{/each}
	</div>

	<button on:click={validate}>Next</button>
</div>

<style lang="postcss">
	input:not([type='checkbox']),
	select {
		@apply block w-full;
	}

	.c {
		@apply inline-flex items-center space-x-1;
	}

	.c span {
		@apply order-2;
	}

	.c span:not(.d) {
		@apply cursor-pointer select-none order-1 pr-2;
	}

	input[type='checkbox'] {
		@apply w-0 h-0;
	}
</style>
