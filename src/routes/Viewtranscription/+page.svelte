<script lang="ts">
	import { writable } from 'svelte/store';

	// Define initial data for transcriptions
	interface Transcription {
		speaker: string;
		text: string;
		audioUrl: string;
	}

	// Mock transcription data
	let transcriptions: Transcription[] = [
		{ speaker: 'SPEAKER_00', text: 'Transcription text here...', audioUrl: '#' },
		{ speaker: 'SPEAKER_01', text: 'Another transcription text...', audioUrl: '#' },
		// Add more data as needed
	];

	// Writable store for managing transcription data
	const transcriptionStore = writable(transcriptions);

	// Function to handle actions (like Revisar, Revertir, Dividir, Combinar)
	function handleAction(action: string, index: number) {
		console.log(`${action} clicked on item ${index}`);
		// Add your logic here for each action
	}
</script>

<section class="px-10 py-16">
	<div class="container mx-auto max-w-7xl bg-white shadow-md rounded-lg p-8">
		<!-- Header Section -->
		<h1 class="text-4xl font-bold text-gray-800 mb-6">Transcripción Completa</h1>
		<div class="flex justify-between mb-6">
			<!-- Main Buttons -->
			<div class="space-x-4">
				<button class="btn btn-primary px-8 py-3 rounded-md">Guardar cambios</button>
				<button class="btn btn-secondary px-8 py-3 rounded-md">Combinar segmentos seleccionados</button>
			</div>
		</div>

		<!-- Interlocutors Section -->
		<div class="mb-8">
			<h2 class="text-xl font-medium text-gray-800">Interlocutores</h2>
			<p class="text-sm text-gray-600">Interlocutor SPEAKER_00: <strong>SPEAKER_00</strong></p>
			<p class="text-sm text-gray-600">Interlocutor SPEAKER_01: <strong>SPEAKER_01</strong></p>
		</div>

		<!-- Transcription Table -->
		<div class="overflow-x-auto border border-gray-200 rounded-lg">
			<table class="min-w-full table-auto">
				<thead class="bg-gray-50">
				<tr>
					<th class="px-6 py-3 text-left text-gray-600 font-medium">Interlocutor</th>
					<th class="px-6 py-3 text-left text-gray-600 font-medium">Transcripción</th>
					<th class="px-6 py-3 text-left text-gray-600 font-medium">Acciones</th>
				</tr>
				</thead>
				<tbody class="bg-white divide-y divide-gray-200">
				{#each $transcriptionStore as { speaker, text, audioUrl }, index}
					<tr class="hover:bg-gray-50">
						<td class="px-6 py-4 text-sm font-medium text-gray-800">{speaker}</td>
						<td class="px-6 py-4">
							<div class="flex items-center space-x-3">
								<audio controls src={audioUrl} class="w-40 h-10"></audio>
								<input
									type="text"
									class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
									bind:value={text}
								/>
							</div>
						</td>
						<td class="px-6 py-4">
							<div class="flex space-x-2">
								<button
									class="btn btn-sm btn-primary"
									on:click={() => handleAction('Revisar', index)}
								>
									Revisar
								</button>
								<button
									class="btn btn-sm btn-secondary"
									on:click={() => handleAction('Revertir', index)}
								>
									Revertir
								</button>
								<button
									class="btn btn-sm btn-warning"
									on:click={() => handleAction('Dividir', index)}
								>
									Dividir
								</button>
								<button
									class="btn btn-sm btn-success"
									on:click={() => handleAction('Combinar', index)}
								>
									Combinar
								</button>
							</div>
						</td>
					</tr>
				{/each}
				</tbody>
			</table>
		</div>
	</div>
</section>
