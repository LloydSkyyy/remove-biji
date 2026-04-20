<script lang="ts">
	import ImagePickerCard from '$lib/components/cards/image-picker-card.svelte';
	import ResultCard from '$lib/components/cards/result-card.svelte';

	let images = $state<FileList>();
	let imagePreviews = $derived(
		images && Array.from(images).map((image) => URL.createObjectURL(image))
	);

	let outputs = $state<Blob[]>([]);
	const outputPreviews = $derived(
		outputs && Array.from(outputs).map((output) => URL.createObjectURL(output))
	);

	let isLoading = $state(false);

	$effect(() => {
		if (!images || images.length === 0) {
			outputs = [];
			outputPreviews.forEach(URL.revokeObjectURL);
		}
	});
</script>

<svelte:head>
	<title>{isLoading ? '[Processing...] ' : ''}Remove BG - Hilangkan backgroundmu dengan mudah</title>
</svelte:head>

<div class="mt-4 flex flex-wrap items-start gap-4 md:flex-nowrap">
	<ImagePickerCard bind:images {imagePreviews} bind:outputs bind:isLoading />
	<ResultCard {isLoading} {outputs} {outputPreviews} />
</div>
