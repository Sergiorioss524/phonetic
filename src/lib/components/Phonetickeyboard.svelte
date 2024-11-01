
<script lang="ts">
	export let onInsert: (symbol: string) => void;

	const phoneticSymbols = [
		'ə', 'θ', 'ɪ', 'ʊ', 'ɔ', 'æ', 'ʌ', 'ɒ', 'ŋ', 'ʃ', 'ʒ', 'ɡ', 'ɑ', 'eɪ', 'aɪ', 'oʊ', 'ɔɪ', 'p', 'b'
	];
</script>

<div class="phonetic-keyboard grid grid-cols-5 gap-2 p-4 bg-white border rounded-lg shadow-md">
	{#each phoneticSymbols as symbol}
		<button
			class="btn btn-sm bg-gray-100 hover:bg-gray-200 rounded-md"
			on:click={() => onInsert(symbol)}
		>
			{symbol}
		</button>
	{/each}
</div>

<style>
    .phonetic-keyboard {
        max-width: 200px;
    }
</style>
