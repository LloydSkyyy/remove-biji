<script lang="ts">
	import Heading from '$lib/components/heading.svelte';
	import { packages } from '$lib/constants/price';
	import BijiIcon from '$lib/icons/biji-icon.svelte';
	import { formatRupiah } from '$lib/utils';

	const title = 'Pricing - Remove Biji';
	const description = 'Pricing Remove Biji';
</script>

<svelte:head>
	<title>{title}</title>
</svelte:head>

<Heading title="Pricing" subtitle="91% lebih murah dibanding remove.bg" />

<!-- Responsive Container (recommended) -->
<div class="table-container mt-10">
	<!-- Native Table Element -->
	<table class="table table-hover mx-auto max-w-xl">
		<thead>
			<tr>
				<th>Jumlah biji*</th>
				<th>Harga**</th>
				<th>Harga per biji</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td class="flex items-center font-medium">
					<BijiIcon />
					3 / hari***
				</td>
				<td>Gratis</td>
				<td>Gratis</td>
			</tr>
			{#each packages as row}
				<tr>
					<td class="flex items-center font-medium">
						<BijiIcon />
						{row.value}
					</td>
					<td>{formatRupiah(row.totalPrice)}</td>
					<td>{formatRupiah(row.pricePerImage)} / biji</td>
				</tr>
			{/each}
		</tbody>
		<!-- <tfoot>
			<tr>
				<th colspan="3">Calculated Total Weight</th>
				<td>{totalWeight}</td>
			</tr>
		</tfoot> -->
	</table>
</div>

<div class="mt-6 text-center">
	<p class="flex items-center justify-center text-gray-600 dark:text-gray-400">
		*1 <BijiIcon />
		= 1 gambar
	</p>
	<p class="flex items-center justify-center text-gray-600 dark:text-gray-400">
		**Belum termasuk biaya admin
	</p>
	<p class="flex items-center justify-center text-gray-600 dark:text-gray-400">***Non-akumulatif</p>
</div>
