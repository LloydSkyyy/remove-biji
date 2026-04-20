<script lang="ts">
	import TrashIcon from '$lib/icons/trash-icon.svelte';
	import { base64ToBlob, removeFileFromList } from '$lib/utils';
	import clsx from 'clsx';
	import { flip } from 'svelte/animate';
	import { fade } from 'svelte/transition';
	import { match } from 'ts-pattern';
	import FileDropzone from '../file-dropzone.svelte';
	import ResetIcon from '$lib/icons/reset-icon.svelte';
	import { removeBg } from '$lib/remove-bg';
	import { z } from 'zod';
	import BgReplaceIcon from '$lib/icons/bg-replace-icon.svelte';
	import { withCatch } from '@tfkhdyt/with-catch';
	import { getCreditsStore } from '$lib/stores/credit.svelte';

	type Props = {
		images: FileList | undefined;
		imagePreviews: string[] | undefined;
		outputs: Blob[];
		isLoading: boolean;
	};

	let {
		images = $bindable(),
		imagePreviews,
		outputs = $bindable(),
		isLoading = $bindable()
	}: Props = $props();
	const isImageExist = $derived(images && images.length > 0);

	const creditStore = getCreditsStore();

	let statusMessage = '';
	let statusType: 'success' | 'error' | '' = '';
	const canMultiple = $derived(creditStore.amount ? creditStore.amount > 0 : false);

	$effect(() => {
		if (!images || images.length === 0) {
			statusMessage = '';
			statusType = '';
		}
	});

	async function handleRemoveBg() {
		isLoading = true;
		outputs = [];

		const [err, data] = await withCatch(removeBg(images));
		if (err) {
			statusType = 'error';
			statusMessage = err instanceof z.ZodError ? err.issues[0].message : err.message;
			isLoading = false;
			return;
		}

		await Promise.allSettled(
			data!.images!.map(async (image) => {
				const blob = base64ToBlob(image);
				outputs.push(blob);
			})
		);

		creditStore.setAmount(data?.creditsAmount ?? 0);

		statusType = 'success';
		statusMessage = 'Proses berhasil';

		isLoading = false;
	}
</script>

<div class="card w-full md:w-1/2">
	<header class="h4 card-header font-medium">
		<label for="file-upload">Pilih gambar</label>
	</header>
	<section class="max-h-[600px] overflow-y-auto p-4">
		{#if images && images.length > 0}
			<div
				class={clsx(
					'grid gap-4',
					match(images.length)
						.with(1, () => 'grid-cols-1')
						.otherwise(() => 'grid-cols-4')
				)}
			>
				{#each images as image, index (image.name)}
					<div
						class={clsx('group relative mx-auto max-h-[450px]')}
						animate:flip={{ duration: 400 }}
						transition:fade={{ duration: 400 }}
					>
						<div class="relative">
							<img
								src={imagePreviews?.at(index)}
								alt={image.name}
								class={clsx(
									'max-h-[450px] rounded-lg',
									images.length === 1 ? 'object-contain' : 'aspect-square object-cover'
								)}
							/>
							{#if outputs.length === 0 && !isLoading}
								<div
									class={clsx(
										'absolute inset-0 rounded-lg opacity-0 transition-opacity duration-200',
										'bg-gradient-to-bl from-slate-900/80 via-transparent to-transparent',
										'group-hover:opacity-100'
									)}
								></div>
								<button
									class="absolute right-2 top-2 z-20 hidden text-white transition-all hover:text-red-500 group-hover:block"
									onclick={() => (images = images ? removeFileFromList(image, images) : undefined)}
								>
									<TrashIcon class="size-6" />
								</button>
							{/if}
						</div>
					</div>
				{/each}
			</div>
		{:else}
			<div>
				<FileDropzone bind:images multiple={canMultiple} />
			</div>
		{/if}
	</section>
	{#if isImageExist}
		<footer class={clsx('card-footer flex justify-evenly pt-4 text-center')}>
			<button
				type="button"
				class="variant-filled-surface btn"
				onclick={() => {
					images = undefined;
					outputs = [];
					imagePreviews?.forEach(URL.revokeObjectURL);
				}}
				disabled={isLoading}
			>
				<ResetIcon class="mr-2 size-5" />
				Reset</button
			>
			{#if outputs.length === 0}
				<button
					type="button"
					class="plausible-event-name=Remove+Biji variant-filled btn"
					onclick={handleRemoveBg}
					disabled={isLoading}
				>
					<BgReplaceIcon class="mr-2 size-6" />
					Remove the biji</button
				>
			{/if}
		</footer>
	{/if}
	{#if statusMessage}
		<div class="p-4 text-center">
			<p class={clsx(statusType === 'error' ? 'text-red-500' : 'text-green-600')}>
				{statusMessage}
			</p>
		</div>
	{/if}
</div>
