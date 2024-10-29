<script lang="ts">
	import { writable } from 'svelte/store';
	import { faEye, faUndo, faCut, faCompressArrowsAlt, faTrashAlt } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';

	interface Transcription {
		speaker: string;
		text: string;
		audioUrl: string;
		duration: string;
		selected?: boolean;
	}

	// Mock transcription data
	let transcriptions: Transcription[] = [
		{ speaker: 'SPEAKER_00', text: 'Transcription text here...', audioUrl: '#', duration: '00:00', selected: false },
		{ speaker: 'SPEAKER_01', text: 'Another transcription text...', audioUrl: '#', duration: '00:00', selected: false },
	];

	// Writable store for managing transcription data
	const transcriptionStore = writable(transcriptions);

	// Writable stores for speaker names
	let speakerName1 = writable('Carlos');
	let speakerName2 = writable('Maria');

	// Function to auto-resize textarea
	function autoResize(event: Event) {
		const target = event.target as HTMLTextAreaElement;
		target.style.height = 'auto';
		target.style.height = `${target.scrollHeight}px`;
	}


	// Function to save speaker names
	function saveSpeakerNames() {
		speakerName1.set($speakerName1);
		speakerName2.set($speakerName2);
		console.log("Speaker names saved:", $speakerName1, $speakerName2);
	}

	// Function to handle actions (like Revisar, Revertir, Dividir, Combinar)
	function handleAction(action: string, index: number) {
		console.log(`${action} clicked on item ${index}`);
	}

	// Function to handle speaker changes in dropdown
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

	// Helper function to format duration in mm:ss
	function formatDuration(seconds: number): string {
		const minutes = Math.floor(seconds / 60);
		const remainingSeconds = Math.floor(seconds % 60);
		return `${String(minutes).padStart(2, '0')}:${String(remainingSeconds).padStart(2, '0')}`;
	}

	// Function to update duration in the transcription
	function updateDurationFromEvent(index: number, event: Event) {
		const audioElement = event.target as HTMLAudioElement;
		transcriptionStore.update((transcriptions) => {
			transcriptions[index].duration = formatDuration(audioElement.duration);
			return transcriptions;
		});
	}

	// Function to toggle segment selection for combining
	function toggleSegmentSelection(index: number) {
		transcriptionStore.update((transcriptions) => {
			transcriptions[index].selected = !transcriptions[index].selected;
			return transcriptions;
		});
	}

	// Computed property to check if two or more segments are selected
	$: hasTwoOrMoreSelected = $transcriptionStore.filter(segment => segment.selected).length >= 2;
</script>

<section class="px-10 py-16">
	<div class="container mx-auto max-w-7xl bg-white shadow-md rounded-lg p-8">
		<!-- Header Section -->
		<h1 class="text-4xl font-bold text-gray-800 mb-6">Transcripción Completa</h1>
		<div class="flex justify-between mb-6">
			<!-- Main Buttons -->
			<div class="space-x-4">
				<button class="btn btn-primary px-8 py-3 rounded-md" on:click={saveSpeakerNames}>Guardar cambios</button>
				{#if hasTwoOrMoreSelected}
					<button class="btn btn-secondary px-8 py-3 rounded-md">Combinar segmentos seleccionados</button>
				{/if}
			</div>
		</div>

		<!-- Interlocutors Section -->
		<div class="mb-8">
			<h2 class="text-xl font-medium text-gray-800">Interlocutores</h2>
			<div class="flex items-center space-x-2 mb-2">
				<p class="text-sm text-gray-600">Interlocutor SPEAKER_00:</p>
				<input
					type="text"
					bind:value={$speakerName1}
					class="text-gray-800 font-semibold border-b-2 border-gray-300 focus:border-indigo-500 focus:outline-none"
					placeholder="Enter name"
				/>
			</div>
			<div class="flex items-center space-x-2">
				<p class="text-sm text-gray-600">Interlocutor SPEAKER_01:</p>
				<input
					type="text"
					bind:value={$speakerName2}
					class="text-gray-800 font-semibold border-b-2 border-gray-300 focus:border-indigo-500 focus:outline-none"
					placeholder="Enter name"
				/>
			</div>
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
				{#each $transcriptionStore as segment, index}
					<tr class="{segment.selected ? 'bg-gray-200' : ''}">
						<td class="px-4 py-4 text-sm font-medium text-gray-800">
							<!-- Dropdown to select speaker -->
							<select on:change={(e) => handleSelectChange(index, e)} class="border rounded-lg px-2 py-1">
								<option value="SPEAKER_00" selected={segment.speaker === 'SPEAKER_00'}>Carlos</option>
								<option value="SPEAKER_01" selected={segment.speaker === 'SPEAKER_01'}>Maria</option>
							</select>
						</td>
						<td class="px-4 py-4">
							<div class="flex items-center space-x-3">
								<audio
									controls
									src={segment.audioUrl}
									class="w-40 h-10"
									on:loadedmetadata={(e) => updateDurationFromEvent(index, e)}
								></audio>
								<span class="text-sm text-gray-500">{segment.duration}</span>
								<textarea
									class="w-full px-3 py-2 border rounded-lg resize-none focus:outline-none focus:ring-2 focus:ring-indigo-500"
									bind:value={segment.text}
									rows="1"
									style="overflow:hidden;"
									on:input={autoResize}
								></textarea>
							</div>
						</td>
						<td class="px-7 py-4">
							<div class="flex space-x-2 justify-center">
								<button class="btn btn-sm btn-primary" on:click={() => handleAction('Revisar', index)}>
									<Fa icon={faEye} class="h-5 w-5" />
								</button>
								<button class="btn btn-sm btn-secondary" on:click={() => handleAction('Revertir', index)}>
									<Fa icon={faUndo} class="h-5 w-5" />
								</button>
								<button class="btn btn-sm btn-warning" on:click={() => handleAction('Dividir', index)}>
									<Fa icon={faCut} class="h-5 w-5" />
								</button>
								<button class="btn btn-sm btn-success" on:click={() => toggleSegmentSelection(index)}>
									<Fa icon={faCompressArrowsAlt} class="h-5 w-5" />
								</button>
								<button class="btn btn-sm btn-danger" on:click={() => handleAction('Eliminar', index)}>
									<Fa icon={faTrashAlt} class="h-5 w-5" />
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
