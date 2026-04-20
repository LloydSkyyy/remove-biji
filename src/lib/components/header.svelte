<script lang="ts">
	import BillIcon from '$lib/icons/bill-icon.svelte';
	import BuyIcon from '$lib/icons/buy-icon.svelte';
	import CircleIcon from '$lib/icons/circle-icon.svelte';
	import HamburgerIcon from '$lib/icons/hamburger-icon.svelte';
	import InvoiceIcon from '$lib/icons/invoice-icon.svelte';
	import LoadingIcon from '$lib/icons/loading-icon.svelte';
	import LogoutIcon from '$lib/icons/logout-icon.svelte';
	import { getCreditsStore } from '$lib/stores/credit.svelte';
	import type { PopupSettings } from '@skeletonlabs/skeleton';
	import { Avatar, LightSwitch, popup } from '@skeletonlabs/skeleton';
	import { onMount } from 'svelte';
	import { setupViewTransition } from 'sveltekit-view-transition';
	import GoogleLoginButton from './buttons/google-login-button.svelte';
	import Sidebar from './sidebar.svelte';
	import { page } from '$app/stores';
	import BijiIcon from '$lib/icons/biji-icon.svelte';

	type Props = {
		user: {
			id: number;
			name: string | null;
			email: string;
			picture: string | null;
			creditsAmount: number | null;
		} | null;
	};

	let { user }: Props = $props();

	const creditStore = getCreditsStore();

	onMount(() => {
		creditStore.setAmount(user?.creditsAmount ?? 0);
	});

	const profilePopup: PopupSettings = {
		// Represents the type of event that opens/closed the popup
		event: 'click',
		// Matches the data-popup value on your popup element
		target: 'profilePopup',
		// Defines which side of your trigger the popup will appear
		placement: 'bottom'
	};

	const { transition } = setupViewTransition();

	let isOpen = $state(false);

	function handleToggle() {
		isOpen = !isOpen;
	}

	$effect(() => {
		if ($page) {
			isOpen = false;
		}
	});
</script>

<header class="flex items-center justify-between pb-4" use:transition={'header'}>
	<div>
		<a class="h2 flex items-center font-bold" href="/">
			<div class="mr-2 flex items-center">
				<CircleIcon class="size-8" />
				<CircleIcon class="-ml-3 size-8" />
			</div>
			Remove BG
		</a>
		<h2 class="h6 text-gray-600 dark:text-gray-400">Hilangkan background dengan mudah</h2>
	</div>
	<div class="hidden items-center space-x-8 lg:flex">
		{#if user}
			<span class="flex items-center">
				Jumlah bijimu:
				<BijiIcon />
				{#if creditStore.amount !== null}
					{creditStore.amount}
				{:else}
					<LoadingIcon class="size-4 animate-spin" />
				{/if}
			</span>
			<a href="/topup" class="flex items-center hover:text-gray-600 dark:hover:text-gray-300">
				<BuyIcon class="mr-2 size-5" />
				Top Up</a
			>
			<a href="/invoices" class="flex items-center hover:text-gray-600 dark:hover:text-gray-300">
				<InvoiceIcon class="mr-2 size-5" />
				Purchases</a
			>
		{/if}
		<a href="/pricing" class="flex items-center hover:text-gray-600 dark:hover:text-gray-300">
			<BillIcon class="mr-2 size-5" />
			Pricing</a
		>
		{#if user}
			<div use:popup={profilePopup}>
				<Avatar src={user.picture ?? undefined} width="w-10" rounded="rounded-full" />
			</div>
		{:else}
			<GoogleLoginButton />
		{/if}
		<LightSwitch />
	</div>
	<div class="flex items-center space-x-4 lg:hidden">
		{#if user}
			<div class="flex items-center">
				<BijiIcon />
				{#if creditStore.amount !== null}
					{creditStore.amount}
				{:else}
					<LoadingIcon class="size-4 animate-spin" />
				{/if}
			</div>
		{/if}

		<button onclick={handleToggle} aria-label="Menu"> <HamburgerIcon class="size-10" /> </button>
	</div>
</header>

<!-- profile popup -->
<div class="card z-50 max-w-56 p-4 shadow-xl" data-popup="profilePopup">
	<div>
		<p>Hello, {user?.name}</p>

		<a href="/auth/logout" class="variant-ghost-error btn mt-4 w-full">
			<LogoutIcon class="mr-2 size-5" />
			Logout</a
		>
	</div>
	<div class="bg-surface-100-800-token arrow"></div>
</div>

<Sidebar {user} {isOpen} {handleToggle} />
