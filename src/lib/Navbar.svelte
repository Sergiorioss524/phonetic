<script lang="ts">
	import { writable } from 'svelte/store';
	import { Dialog } from '@rgossiaux/svelte-headlessui';
	import { page } from '$app/stores';
	import PageLogo from '$lib/components/PageLogo.svelte';
	import Fa from 'svelte-fa';
	import { faBars, faTimes } from '@fortawesome/free-solid-svg-icons';
	import {faXmark} from '@fortawesome/free-solid-svg-icons';

	// Access the pathname
	$: showConvertButton = $page.url.pathname.startsWith('/convert') === false;

	const navigation = [
		{ name: 'Features', href: '/features' },
		{ name: 'Contact', href: '/contact' },
		{ name: 'Terms', href: '/terms' },
	];

	const mobileMenuOpen = writable(false);

	function closeMobileMenu() {
		$mobileMenuOpen = false;
	}

	function openMobileMenu() {
		$mobileMenuOpen = true;
	}
</script>

<header class="relative z-20">
	<nav
		class="mx-auto flex max-w-7xl items-center justify-between gap-x-4 p-3 sm:gap-x-6 sm:p-6 lg:px-8"
		aria-label="Global"
	>
		<div class="flex lg:flex-1">
			<PageLogo />
		</div>
		<div class="hidden lg:flex lg:gap-x-12">
			{#each navigation as item (item.name)}
				<a href={item.href} class="text-sm font-semibold leading-6">{item.name}</a>
			{/each}
		</div>
		<div
			class="flex-1 items-center justify-end gap-x-6 {!showConvertButton
        ? 'hidden sm:flex'
        : 'flex'} hidden lg:flex"
		>
			<a href="/transcription" class="btn btn-primary btn-sm rounded-md">Pruebalo ahora</a>
			<a href="/#" class="w-fit h-fit p-0 border-b-0 no-underline text-sm leading-5 below-sm:text-xs below-sm:leading-[1.125rem]">
				<button class="font-medium py-1.5 px-3 bg-transparent hover:bg-base-300 hover:rounded-md flex items-center">
					Sign In
					<svg class="ml-1 w-2.5 h-2.5" viewBox="0 0 24 24" stroke-width="3" fill="none" xmlns="http://www.w3.org/2000/svg" color="currentColor">
						<path d="M3 12L21 12M21 12L12.5 3.5M21 12L12.5 20.5" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"></path>
					</svg>
				</button>
			</a>
		</div>
		<div class="flex lg:hidden">
			<button
				type="button"
				class="-m-2.5 inline-flex items-center justify-center rounded-md p-2.5 text-base-content"
				on:click={openMobileMenu}
			>
				<span class="sr-only">Open main menu</span>
				<Fa icon={faBars} class="h-6 w-6" />
			</button>
		</div>
	</nav>

	<Dialog on:close={closeMobileMenu} bind:open={$mobileMenuOpen} class="fixed inset-0 z-50">
		<div
			class="fixed inset-y-0 right-0 z-10 w-full overflow-y-auto bg-base-200 p-3 sm:max-w-sm sm:ring-1 sm:ring-base-content/10"
		>
			<div class="flex items-center justify-between gap-x-6">
				<PageLogo />
				<button
					type="button"
					class="-m-2.5 rounded-md p-2.5 text-base-content"
					on:click={closeMobileMenu}
				>
					<span class="sr-only">Close menu</span>
					<Fa icon={faXmark} class="h-6 w-6" />
				</button>
			</div>
			<div class="mt-6 flow-root">
				<div class="-my-6 divide-y divide-gray-500/10 px-2">
					<div class="space-y-2 pt-6">
						{#each navigation as item (item.name)}
							<a
								href={item.href}
								class="-mx-3 block rounded-lg px-3 py-2 text-base font-semibold leading-7 text-base-content hover:bg-gray-50"
							>{item.name}</a>
						{/each}
					</div>
					<hr class="my-5 border border-gray-600" />
					<a href="/#" class="btn btn-primary btn-sm btn-block lg:hidden">Pruebalo ahora</a>
					<a href="/#" class="btn btn-accent btn-sm btn-block mt-2 lg:hidden">login</a>
				</div>
			</div>
		</div>
	</Dialog>
</header>
