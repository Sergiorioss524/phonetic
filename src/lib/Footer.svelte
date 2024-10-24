<script>
	import PageLogo from '$lib/components/PageLogo.svelte';

	const navigation = {
		main: [
			{ name: 'Features', href: '/features' },
			{ name: 'Contact', href: '/contact' },
			{ name: 'Terms', href: '/terms' },
		],
	};
</script>

<footer class="m-4 rounded-lg bg-base-300 px-8">
	<div class="mx-auto w-full max-w-screen-xl p-4 md:py-8">
		<div class="sm:flex sm:items-center sm:justify-between">
			<PageLogo />
			{#each navigation.main as item (item.name)}
				<div>
					<a href={item.href} class="text-sm leading-6 hover:opacity-75">{item.name}</a>
				</div>
			{/each}
		</div>
		<hr class="my-6 border-gray-200 dark:border-gray-700 sm:mx-auto lg:my-8" />
		<span class="block text-sm text-gray-500 dark:text-gray-400 sm:text-center">
      © 2023 <a href="/" class="hover:underline">SpeechSolutions </a>. All Rights Reserved.
    </span>
	</div>
</footer>
