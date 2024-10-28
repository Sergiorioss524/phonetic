<script lang="ts">
	import { writable } from 'svelte/store';
	import { faEye, faUndo, faCut, faCompressArrowsAlt } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';

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
	}

	// Function to handle speaker changes
	function changeSpeaker(index: number, value: string) {
		transcriptionStore.update((transcriptions) => {
			transcriptions[index].speaker = value;
			return transcriptions;
		});
	}

	// Handler function to process the change event with type casting
	function handleSelectChange(index: number, event: Event) {
		const target = event.target as HTMLSelectElement;
		changeSpeaker(index, target.value);
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
			<p class="text-sm text-gray-600">Interlocutor SPEAKER_00: <strong>Carlos</strong></p>
			<p class="text-sm text-gray-600">Interlocutor SPEAKER_01: <strong>Maria</strong></p>
		</div>

		<!-- Transcription Table -->
		<div class="overflow-x-auto border border-gray-200 rounded-lg">
			<table class="min-w-full table-auto">
				<thead class="bg-gray-50">
				<tr>
					<th class="px-4 py-3 text-left text-gray-600 font-medium w-1/6">Interlocutor</th>
					<th class="px-4 py-3 text-left text-gray-600 font-medium w-4/6">Transcripción</th>
					<th class="px-2 py-3 text-left text-gray-600 font-medium w-1/6">Acciones</th>
				</tr>
				</thead>
				<tbody class="bg-white divide-y divide-gray-200">
				{#each $transcriptionStore as { speaker, text, audioUrl }, index}
					<tr class="hover:bg-gray-50">
						<td class="px-4 py-4 text-sm font-medium text-gray-800">
							<!-- Dropdown to select speaker -->
							<select on:change={(e) => handleSelectChange(index, e)} class="border rounded-lg px-2 py-1">
								<option value="SPEAKER_00" selected={speaker === 'SPEAKER_00'}>Carlos</option>
								<option value="SPEAKER_01" selected={speaker === 'SPEAKER_01'}>Maria</option>
							</select>
						</td>
						<td class="px-4 py-4">
							<div class="flex items-center space-x-3">
								<audio controls src={audioUrl} class="w-40 h-10"></audio>
								<input
									type="text"
									class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
									bind:value={text}
								/>
							</div>
						</td>
						<td class="px-7 py-4">
							<div class="flex space-x-2 justify-center">
								<!-- Icon Buttons with Tooltips -->
								<button
									class="btn btn-sm btn-primary relative group"
									on:click={() => handleAction('Revisar', index)}
								>
									<Fa icon={faEye} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                      Revisar
                    </span>
								</button>

								<button
									class="btn btn-sm btn-secondary relative group"
									on:click={() => handleAction('Revertir', index)}
								>
									<Fa icon={faUndo} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                      Revertir
                    </span>
								</button>

								<button
									class="btn btn-sm btn-warning relative group"
									on:click={() => handleAction('Dividir', index)}
								>
									<Fa icon={faCut} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                      Dividir
                    </span>
								</button>

								<button
									class="btn btn-sm btn-success relative group"
									on:click={() => handleAction('Combinar', index)}
								>
									<Fa icon={faCompressArrowsAlt} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                      Combinar
                    </span>
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
