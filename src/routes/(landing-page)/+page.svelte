<script lang="ts">
	import { setupViewTransition } from 'sveltekit-view-transition';
	import { Accordion, AccordionItem } from '@skeletonlabs/skeleton';
	import CompareImage from '$lib/components/compare-image.svelte';
	import Icon from '@iconify/svelte';

	const features = [
		{
			icon: 'lucide:zap',
			title: 'Proses Instan',
			description: 'Hasil dalam hitungan detik dengan teknologi AI terkini'
		},
		{
			icon: 'majesticons:image-line',
			title: 'Kualitas HD',
			description: 'Hasil berkualitas tinggi tanpa mengurangi resolusi asli'
		},
		{
			icon: 'lucide:circle-check-big',
			title: 'Mudah Digunakan',
			description: 'Interface sederhana, upload dan download dengan mudah'
		},
		{
			icon: 'lucide:badge-percent',
			title: 'Harga terjangkau',
			description: '91% lebih murah dibanding remove.bg'
		}
	];

	const caraKerja = [
		{
			icon: 'material-symbols:upload',
			title: 'Upload Gambar',
			description: 'Drag & drop atau pilih gambar Anda'
		},
		{
			icon: 'uis:process',
			title: 'Proses Otomatis',
			description: 'AI kami akan memproses gambar Anda'
		},
		{
			icon: 'material-symbols:download',
			title: 'Download Hasil',
			description: 'Unduh gambar tanpa background'
		}
	];

	const faq = [
		{
			question: 'Kenapa namanya Remove "Biji" ???',
			answer: 'Background -> (Disingkat jadi) BG -> (Dibaca) Bi-ji -> Biji'
		},
		{
			question: 'Apa perbedaan dengan remove.bg?',
			answer:
				'Free tier remove.bg menghasilkan output yang berkualitas rendah. Sementara itu, free tier remove biji menghasilkan kualitas gambar yang setara dengan gambar asli.'
		},
		{
			question: 'Kenapa saya harus menggunakan aplikasi ini?',
			answer: 'Harga saldo remove biji 91% lebih terjangkau dibanding remove.bg.'
		}
	];

	const { transition } = setupViewTransition();

	const handleBackgroundImage = `url('data:image/svg+xml;utf8,<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M3 12H21M3 12L7 8M3 12L7 16M21 12L17 16M21 12L17 8" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>')`;
</script>

<svelte:head>
	<title>Remove BG - Hilangkan backgroundmu dengan mudah</title>
</svelte:head>

<div class="relative min-h-screen">
	<header class="mx-auto px-4 py-16 lg:container md:px-6 lg:px-8">
		<div class="text-center">
			<h1
				class="mb-4 text-4xl font-bold text-gray-900 md:text-5xl dark:text-gray-50"
				use:transition={'heading'}
			>
				Hapus Background Gambar
				<span class="text-emerald-500"> dalam Sekejap</span>
			</h1>
			<p class="mb-12 text-lg text-gray-600 dark:text-gray-400" use:transition={'subtitle'}>
				Solusi AI yang powerful untuk menghapus background foto Anda secara otomatis dalam hitungan
				detik
			</p>
			<a href="/app" class="variant-filled-primary btn btn-lg font-medium"> Coba Gratis </a>
		</div>
	</header>

	<section
		class="mx-auto flex flex-wrap gap-8 px-4 pb-32 pt-16 lg:container md:px-6 lg:flex-nowrap lg:px-8"
	>
		<div class="rounded lg:w-1/2">
			<CompareImage
				imageLeftSrc="/before.webp"
				imageLeftAlt="left"
				imageRightSrc="/after.webp"
				imageRightAlt="right"
				--handle-size="2.5rem"
				--handle-background-color="rgba(0, 0, 0, 0.6)"
				--handle-background-image={handleBackgroundImage}
				--handle-border-width="0.125rem"
				--slider-color="#ffffff"
				--slider-width="0.125rem"
			/>
		</div>
		<div class="grid gap-8 md:grid-cols-2 lg:w-1/2">
			{#each features as feature}
				<div
					class="space-y-2 rounded-xl bg-surface-100 p-10 shadow-lg transition duration-300 hover:bg-surface-200 dark:bg-surface-800 hover:dark:bg-surface-700"
				>
					<Icon icon={feature.icon} width="3rem" class="text-emerald-500" />
					<h3 class="text-xl font-semibold">{feature.title}</h3>
					<p class="text-gray-600 dark:text-gray-400">{feature.description}</p>
				</div>
			{/each}
		</div>
	</section>

	<section class="mx-auto pb-32 lg:container">
		<div class="mx-auto px-4 lg:container md:px-6 lg:px-8">
			<h2 class="mb-12 text-center text-3xl font-bold">Cara Kerja</h2>
			<div class="grid gap-8 md:grid-cols-3">
				{#each caraKerja as cara, i}
					<div
						class="rounded-xl bg-surface-100 px-4 py-8 text-center shadow-lg transition duration-300 hover:bg-surface-200 dark:bg-surface-800 hover:dark:bg-surface-700"
					>
						<div
							class="mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-emerald-500"
						>
							<Icon icon={cara.icon} width="2rem" class="text-emerald-100 dark:text-emerald-800" />
						</div>
						<h3 class="mb-2 font-semibold">{i + 1}. {cara.title}</h3>
						<p class="text-gray-600 dark:text-gray-400">{cara.description}</p>
					</div>
				{/each}
			</div>
		</div>
	</section>

	<section class="pb-32">
		<div class="mx-auto px-4 lg:container md:px-6 lg:px-8">
			<h2 class="mb-12 text-center text-3xl font-bold">Frequently Asked Questions</h2>
			<Accordion class="mx-auto max-w-2xl space-y-4">
				{#each faq as f}
					<AccordionItem>
						<svelte:fragment slot="summary">
							<span class="font-medium">{f.question}</span>
						</svelte:fragment>
						<svelte:fragment slot="content">
							<span class="text-gray-600 dark:text-gray-400">
								{f.answer}
							</span>
						</svelte:fragment>
					</AccordionItem>
				{/each}
			</Accordion>
		</div>
	</section>

	<section class="bg-surface-100 px-4 py-32 text-center dark:bg-surface-800">
		<h2 class="mb-4 text-3xl font-bold">Siap Mencoba?</h2>
		<p class="mb-8 text-lg text-gray-600 dark:text-gray-400">
			Dapatkan 5 saldo gratis untuk member baru
		</p>
		<a href="/app" class="variant-filled-primary btn btn-lg font-medium"> Coba Sekarang </a>
	</section>
</div>
