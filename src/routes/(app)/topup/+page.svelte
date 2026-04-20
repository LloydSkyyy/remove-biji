<script lang="ts">
	import Heading from '$lib/components/heading.svelte';
	import { packages } from '$lib/constants/price';
	import BijiIcon from '$lib/icons/biji-icon.svelte';
	import BuyIcon from '$lib/icons/buy-icon.svelte';
	import { formatRupiah } from '$lib/utils';
	import clsx from 'clsx';
	import { superForm } from 'sveltekit-superforms';
	import type { PageData } from './$types';

	type Props = {
		data: PageData;
	};

	let { data }: Props = $props();
	const { form, enhance, delayed } = superForm(data.form);

	const title = 'Top Up - Remove Biji';
	const description = 'Top Up Remove Biji';
</script>

<svelte:head>
	<title>{title}</title>
</svelte:head>

<Heading title="Top Up" subtitle="Isi ulang bijimu" />
	keywords="hapus background foto online, remove background gambar, background remover gratis, penghapus background otomatis, edit background foto online, cara menghapus background foto online, aplikasi hapus background foto gratis, remove background gambar HD, background remover tanpa watermark, edit background foto profesional, background remover AI, hapus background foto batch, remove background transparan, edit background foto bulk, background eraser HD, remove background foto produk, edit background foto profil, hapus background foto dokumen, background remover untuk jualan online, edit background foto KTP, background remover Indonesia, hapus background foto bahasa Indonesia, remove background gambar web Indonesia, aplikasi background remover terbaik, software hapus background tercepat, background remover vs photoshop, alternatif remove.bg, remove background png, edit background foto cepat, background remover untuk marketplace, hapus background foto sekali klik, aplikasi edit background terpopuler, cara edit background foto online gratis, edit background foto resolution tinggi, background remover untuk pemula"
	openGraph={{
		title,
		description,
		type: 'website',
		site_name: 'Remove Biji'
	}}
	twitter={{
		card: 'summary',
		site: '@tfkhdyt__',
		title,
		description
	}}
/>

<Heading title="Top Up" subtitle="Isi ulang bijimu" />

<form method="POST" use:enhance>
	<div class="mt-10 flex justify-center">
		<div class="space-y-4">
			{#each packages as row, i}
				<label
					class={clsx(
						'flex items-center space-x-2',
						row.value === $form.package || 'text-gray-600 dark:text-gray-400'
					)}
				>
					<input
						class="radio"
						type="radio"
						name="package"
						bind:group={$form.package}
						value={row.value}
						required={i === 0}
					/>
					<span class="flex w-[75svw] items-center justify-between md:w-[250px]">
						<span class="-ml-2 flex items-center">
							<BijiIcon />
							{row.value}
						</span>
						<span>{formatRupiah(row.totalPrice)}</span>
					</span>
				</label>
			{/each}
		</div>
	</div>

	<div class="mt-6 space-y-5 text-center">
		<p class="flex items-center justify-center text-gray-600 dark:text-gray-400">
			1 <BijiIcon />
			= 1 gambar
		</p>

		<button
			type="submit"
			class="plausible-event-name=Top+Up variant-filled btn"
			disabled={$delayed || !$form.package}
		>
			<BuyIcon class="mr-2 size-5" />
			Beli sekarang (QRIS)
		</button>
	</div>
</form>
