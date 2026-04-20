<script lang="ts">
	import { env } from '$env/dynamic/public';
	import { arrow, autoUpdate, computePosition, flip, offset, shift } from '@floating-ui/dom';
	import '@fontsource/roboto/100.css';
	import '@fontsource/roboto/300.css';
	import '@fontsource/roboto/400.css';
	import '@fontsource/roboto/500.css';
	import '@fontsource/roboto/700.css';
	import '@fontsource/roboto/900.css';
	import { storePopup } from '@skeletonlabs/skeleton';
	import type { Snippet } from 'svelte';
	import { setupViewTransition } from 'sveltekit-view-transition';
	import '../app.css';

	type Props = {
		children: Snippet<[]>;
	};

	let { children }: Props = $props();

	const title = 'Remove BG - Hilangkan backgroundmu dengan mudah';
	const description =
		'Remove BG adalah aplikasi background remover minimalis yang mudah digunakan';

	storePopup.set({ computePosition, autoUpdate, offset, shift, flip, arrow });

	setupViewTransition();
</script>


{@render children()}


