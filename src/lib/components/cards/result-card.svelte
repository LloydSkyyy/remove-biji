<script lang="ts">
	import { downloadBlob, downloadAll } from '$lib/downloads';
	import DownloadIcon from '$lib/icons/download-icon.svelte';
	import DownloadMultipleIcon from '$lib/icons/download-multiple-icon.svelte';
	import LoadingIcon from '$lib/icons/loading-icon.svelte';
	import ViewIcon from '$lib/icons/view-icon.svelte';
	import { match } from 'ts-pattern';
	import clsx from 'clsx';
	import { flip } from 'svelte/animate';
	import { fade } from 'svelte/transition';
	import { v4 as uuidv4 } from 'uuid';
	import { withCatch } from '@tfkhdyt/with-catch';

	type Props = {
		outputs: Blob[];
		isLoading: boolean;
		outputPreviews: string[];
	};

	let { outputs, isLoading, outputPreviews }: Props = $props();

	async function handleDownloadAll() {
		const [err] = await withCatch(downloadAll(outputs));
		if (err) {
			console.error(err.message);
		}
	}
</script>

<div class="card w-full md:w-1/2">
	<header class="h4 card-header font-medium">Hasil</header>
	<section class="overflow-y-auto p-4">
		{#if outputs && outputs.length > 0}
			<div
				in:fade={{ duration: 400 }}
				class={clsx(
					'grid gap-4',
					match(outputs.length)
						.with(1, () => 'grid-cols-1')
						.otherwise(() => 'grid-cols-4')
				)}
			>
				<!-- eslint-disable-next-line @typescript-eslint/no-unused-vars -->
				{#each outputs as _, i (i)}
					<div class="max-h-[450px] group relative mx-auto" animate:flip={{ duration: 400 }}>
						<img
							src={outputPreviews[i]}
							alt="output {i + 1}"
							class={clsx(
								'max-h-[450px] rounded-lg',
								outputs.length === 1 ? 'object-contain' : 'aspect-square object-cover'
							)}
						/>
						<div
							class="absolute inset-x-0 bottom-0 items-center space-x-2 p-2 opacity-0 transition-opacity duration-200 group-hover:opacity-100"
						>
							<a
								href={outputPreviews[i]}
								class="plausible-event-name=View variant-filled-surface btn rounded-lg"
								target="_blank"><ViewIcon class="size-5" /></a
							>
							{#if outputs.length > 1}
								<button
									class="plausible-event-name=Download variant-filled btn rounded-lg"
									onclick={() => downloadBlob(outputPreviews[i], `remove-biji-${uuidv4()}.png`)}
									><DownloadIcon class="size-5" /></button
								>
							{/if}
						</div>
					</div>
				{/each}
			</div>
		{:else if isLoading}
			<div class="flex h-full items-center justify-center py-20">
				<div class="flex flex-col items-center justify-center space-y-3">
					<LoadingIcon class="block size-20 animate-spin" />
					<p class="text-center text-black/50 dark:text-white/50">Sabar bang, lagi diproses</p>
				</div>
			</div>
		{:else}
			<div class="flex h-full items-center justify-center py-20">
				<p class="text-center text-black/50 dark:text-white/50">Hasil akan muncul di sini</p>
			</div>
		{/if}
	</section>
	{#if outputs && outputs.length > 0}
		<footer class="card-footer flex pt-4 text-center">
			{#if outputs.length > 1}
				<button
					type="button"
					class="plausible-event-name=Download+Multiple variant-filled btn mx-auto"
					onclick={handleDownloadAll}
				>
					<DownloadMultipleIcon class="mr-2 size-6" />
					Download semua
				</button>
			{:else if outputs.length === 1}
				<button
					type="button"
					class="plausible-event-name=Download variant-filled btn mx-auto"
					onclick={() => downloadBlob(outputPreviews[0], `remove-biji-${uuidv4()}.png`)}
				>
					<DownloadIcon class="mr-2 size-6" />
					Download
				</button>
			{/if}
		</footer>
	{/if}
</div>
