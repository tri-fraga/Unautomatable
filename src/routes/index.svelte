<script>
	import Form1 from '$lib/components/form1.svelte';
	import Form2 from '$lib/components/form2.svelte';

	let progress = 1;

	function trackProgress() {
		progress++;
	}
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<section>
	<div class="py-12">
		<h2 class="text-2xl font-bold cursor-pointer" on:click={() => (progress = 1)}>Automate This</h2>
		<div class="mt-8 max-w-md">
			{#if progress == 1}
				<Form1 on:success={trackProgress} />
			{:else if progress == 2}
				<Form2 on:success={trackProgress} />
			{:else if progress == 3}
				<h1>Congrats, you finished it!</h1>
			{/if}
		</div>
	</div>
</section>

<style lang="postcss">
</style>
