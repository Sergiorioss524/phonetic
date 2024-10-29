<script lang="ts">
	import { writable } from 'svelte/store';
	import { faEye, faUndo, faCut, faCompressArrowsAlt, faTrashAlt, faCheck } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';

	interface Transcription {
		speaker: string;
		text: string;
		audioUrl: string;
		duration: string;
		selected?: boolean;
		revised?: boolean;
	}

	// Mock transcription data
	let transcriptions: Transcription[] = [
		{ speaker: 'SPEAKER_00', text: 'Transcription text here...', audioUrl: '#', duration: '00:00', selected: false, revised:false },
		{ speaker: 'SPEAKER_01', text: 'Another transcription text...', audioUrl: '#', duration: '00:00', selected: false, revised:false },
	];

	// Writable store for managing transcription data
	const transcriptionStore = writable(transcriptions);

	// Writable stores for speaker names
	let speakerName1 = writable('Carlos');
	let speakerName2 = writable('Maria');

	// Temporary variables for modal inputs
	let tempSpeakerName1 = '';
	let tempSpeakerName2 = '';

	// Variable to control modal visibility
	let showRenameModal = false;

	// Function to open the rename modal
	function openRenameModal() {
		tempSpeakerName1 = $speakerName1;
		tempSpeakerName2 = $speakerName2;
		showRenameModal = true;
	}

	// Function to close the rename modal
	function closeRenameModal() {
		showRenameModal = false;
	}

	// Function to save speaker names and close the modal
	function saveSpeakerNames() {
		speakerName1.set(tempSpeakerName1);
		speakerName2.set(tempSpeakerName2);
		console.log("Speaker names saved:", tempSpeakerName1, tempSpeakerName2);
		closeRenameModal();
	}

	// Function to handle actions (like Revisar, Revertir, Dividir, Combinar)
	function handleAction(action: string, index: number) {
		if (action === 'Revisar') {
			transcriptionStore.update((transcriptions) => {
				transcriptions[index].revised = !transcriptions[index].revised;
				return transcriptions;
			});
		} else {
			console.log(`${action} clicked on item ${index}`);
		}
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

	// Function to auto-resize textarea
	function autoResize(event: Event) {
		const target = event.target as HTMLTextAreaElement;
		target.style.height = 'auto';
		target.style.height = `${target.scrollHeight}px`;
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
				<button class="btn btn-primary px-8 py-3 rounded-md" on:click={openRenameModal}>Renombrar interlocutores</button>
				<button class="btn btn-secondary px-8 py-3 rounded-md">Descargar Audio</button>
				<button class="btn btn-secondary px-8 py-3 rounded-md">Renombrar transcripción</button>
				<button class="btn btn-secondary px-8 py-3 rounded-md">Descargar Transcripción</button>
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
				<p class="text-gray-800 font-semibold">{$speakerName1}</p>
			</div>
			<div class="flex items-center space-x-2">
				<p class="text-sm text-gray-600">Interlocutor SPEAKER_01:</p>
				<p class="text-gray-800 font-semibold">{$speakerName2}</p>
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
								<option value="SPEAKER_00" selected={segment.speaker === 'SPEAKER_00'}>{$speakerName1}</option>
								<option value="SPEAKER_01" selected={segment.speaker === 'SPEAKER_01'}>{$speakerName2}</option>
							</select>
						</td>
						<td class="px-4 py-4">
							<div class="flex items-center space-x-3">
								<!-- Custom Play Button with Duration -->
								<button
									class="flex items-center justify-center bg-gray-200 text-black rounded-full px-4 py-2 cursor-pointer"
									style="font-family: 'Courier New', monospace; font-size: 1rem;"
								>
									<!-- Play Icon and Time Text -->
									<span class="mr-2 text-gray-600 text-lg">▶</span>
									<span class="text-black">00:00</span>
								</button>

								<!-- Text Area for Transcription -->
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
								<!-- Tooltip-enabled buttons -->
								<!-- Inside the #each loop, replace the existing "Revisar" button with this code -->
								<button
									class="btn btn-sm relative group {segment.revised ? 'btn-success' : 'btn-primary'}"
									on:click={() => handleAction('Revisar', index)}
								>
									<Fa icon={segment.revised ? faCheck : faEye} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
    {segment.revised ? 'Marcar para revisar' : 'Validar transcripcion'}
  </span>
								</button>

								<button class="btn btn-sm btn-secondary relative group" on:click={() => handleAction('Revertir', index)}>
									<Fa icon={faUndo} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                      Revertir
                    </span>
								</button>
								<button class="btn btn-sm btn-warning relative group" on:click={() => handleAction('Dividir', index)}>
									<Fa icon={faCut} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                      Dividir
                    </span>
								</button>
								<button class="btn btn-sm btn-success relative group" on:click={() => toggleSegmentSelection(index)}>
									<Fa icon={faCompressArrowsAlt} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                      Combinar
                    </span>
								</button>
								<button class="btn btn-sm btn-danger relative group" on:click={() => handleAction('Eliminar', index)}>
									<Fa icon={faTrashAlt} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                      Eliminar
                    </span>
								</button>
							</div>
						</td>
					</tr>
				{/each}
				</tbody>
			</table>
		</div>


		{#if showRenameModal}
			<div class="fixed inset-0 flex items-center justify-center z-50">
				<div class="absolute inset-0 bg-black opacity-50"></div>
				<div class="bg-white rounded-lg overflow-hidden shadow-xl transform transition-all sm:max-w-lg sm:w-full z-50">
					<div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
						<h3 class="text-lg leading-6 font-medium text-gray-900 mb-4">Renombrar Interlocutores</h3>
						<div class="mb-4">
							<label class="block text-gray-700 text-sm font-bold mb-2" for="speakerName1">
								Interlocutor SPEAKER_00:
							</label>
							<input
								id="speakerName1"
								type="text"
								bind:value={tempSpeakerName1}
								class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
								placeholder="Ingrese el nombre"
							/>
						</div>
						<div class="mb-4">
							<label class="block text-gray-700 text-sm font-bold mb-2" for="speakerName2">
								Interlocutor SPEAKER_01:
							</label>
							<input
								id="speakerName2"
								type="text"
								bind:value={tempSpeakerName2}
								class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
								placeholder="Ingrese el nombre"
							/>
						</div>
					</div>
					<div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
						<button
							type="button"
							class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-indigo-600 text-base font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:ml-3 sm:w-auto sm:text-sm"
							on:click={saveSpeakerNames}
						>
							Guardar
						</button>
						<button
							type="button"
							class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
							on:click={closeRenameModal}
						>
							Cancelar
						</button>
					</div>
				</div>
			</div>
		{/if}
	</div>
</section>
