<!-- PhoneticKeyboard.svelte -->
<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher<string>();

	// List of phonetic symbols
	const phoneticSymbols = [
		'ɑ', 'æ', 'ə', 'ɛ', 'ɪ', 'ɒ', 'ʊ', 'ʌ', 'ʃ', 'ʒ', 'θ', 'ð', 'ŋ', 'ʔ',
		'ɜː', 'ɔː', 'uː', 'iː', 'eɪ', 'aɪ', 'ɔɪ', 'əʊ', 'aʊ', 'ɪə', 'eə', 'ʊə'
	];

	// Function to handle symbol click
	function insertSymbol(symbol: string) {
		dispatch('insert', symbol);
	}
</script>

<div
	class="flex flex-wrap bg-gray-100 border border-gray-300 p-2 rounded-md phonetic-keyboard"
>
	{#each phoneticSymbols as symbol}
		<button
			class="m-1 px-2 py-1 bg-white border border-gray-300 rounded hover:bg-gray-200 text-lg"
			on:click={() => insertSymbol(symbol)}
		>
			{symbol}
		</button>
	{/each}
</div>
