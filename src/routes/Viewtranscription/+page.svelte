<script lang="ts">
	import { writable } from 'svelte/store';
	import { faEye, faUndo, faCut, faCompressArrowsAlt, faTrashAlt, faCheck } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';
	import { faUserEdit, faDownload, faEdit, faFileAlt, faObjectGroup } from '@fortawesome/free-solid-svg-icons';

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
		{ speaker: 'SPEAKER_00', text: 'Transcription text here...', audioUrl: '#', duration: '00:00', selected: false, revised: false },
		{ speaker: 'SPEAKER_01', text: 'Another transcription text...', audioUrl: '#', duration: '00:00', selected: false, revised: false },
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

	// Variable to control the visibility of the combine modal
	let showCombineModal = false;

	// Array to store the segments selected for combining
	let segmentsToCombine = [];

	// Variable to control the visibility of the divide modal
	let showDivideModal = false;

	// Variable to store the segment to be divided
	let segmentToDivide = null;

	// Temporary variable to hold the text being edited for division
	let divideText = '';

	// Variable to hold the selected speaker for the new segment
	let selectedSpeakerForDivide = '';

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

	// Function to open the combine modal
	function openCombineModal() {
		// Get the selected segments
		segmentsToCombine = $transcriptionStore.filter(segment => segment.selected);

		// Check if at least two segments are selected
		if (segmentsToCombine.length >= 2) {
			showCombineModal = true;
		} else {
			// Optionally, show an alert or notification
			alert('Seleccione al menos dos segmentos para combinar.');
		}
	}

	// Function to close the combine modal
	function closeCombineModal() {
		showCombineModal = false;
	}

	// Function to combine segments
	function combineSegments() {
		// Combine texts of the selected segments
		const combinedText = segmentsToCombine.map(segment => segment.text).join(' ');

		// Use the speaker of the first selected segment
		const speaker = segmentsToCombine[0].speaker;

		// Create a new combined segment
		const newSegment: Transcription = {
			speaker,
			text: combinedText,
			audioUrl: '#',
			duration: '00:00',
			selected: false,
			revised: false,
		};

		transcriptionStore.update(transcriptions => {
			// Remove selected segments
			transcriptions = transcriptions.filter(segment => !segment.selected);

			// Find the index to insert the new segment
			const firstSelectedIndex = $transcriptionStore.findIndex(segment => segment === segmentsToCombine[0]);

			transcriptions.splice(firstSelectedIndex, 0, newSegment);

			// Reset selection
			transcriptions.forEach(segment => segment.selected = false);

			return transcriptions;
		});

		// Reset selections and close modal
		segmentsToCombine = [];
		showCombineModal = false;
	}

	// Function to handle actions (like Revisar, Revertir, Dividir, Combinar)
	function handleAction(action: string, index: number) {
		if (action === 'Revisar') {
			transcriptionStore.update((transcriptions) => {
				transcriptions[index].revised = !transcriptions[index].revised;
				return transcriptions;
			});
		} else if (action === 'Dividir') {
			// Open the divide modal
			segmentToDivide = $transcriptionStore[index];
			divideText = segmentToDivide.text; // Initialize with the segment's text
			selectedSpeakerForDivide = segmentToDivide.speaker; // Initialize with current speaker
			showDivideModal = true;
		} else {
			console.log(`${action} clicked on item ${index}`);
		}
	}

	// Function to close the divide modal
	function closeDivideModal() {
		showDivideModal = false;
		segmentToDivide = null;
		divideText = '';
		selectedSpeakerForDivide = '';
	}

	// Function to handle division (functionality to be added later)
	function divideSegment() {
		// Placeholder for division logic
		console.log('Divide segment at specified point');
		closeDivideModal();
	}

	// Function to handle speaker changes in dropdown in divide modal
	function handleSpeakerChangeForDivide(event: Event) {
		const target = event.target as HTMLSelectElement;
		selectedSpeakerForDivide = target.value;
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
	<div class="container mx-auto max-w-full lg:max-w-8xl xl:max-w-9xl bg-white shadow-md rounded-lg p-8">
		<!-- Header Section -->
		<h1 class="text-4xl font-bold text-gray-800 mb-6">Transcripción Completa</h1>
		<div class="flex justify-between mb-6">
			<!-- Main Buttons -->
			<div class="flex flex-wrap gap-2">
				<button class="btn btn-secondary flex items-center px-4 py-1 h-8 rounded-md text-sm">
					<Fa icon={faEdit} class="mr-2 h-4 w-4" />
					Renombrar transcripción
				</button>
				<button class="btn btn-secondary flex items-center px-4 py-1 h-8 rounded-md text-sm" on:click={openRenameModal}>
					<Fa icon={faUserEdit} class="mr-2 h-4 w-4" />
					Renombrar interlocutores
				</button>
				<button class="btn btn-secondary flex items-center px-4 py-1 h-8 rounded-md text-sm">
					<Fa icon={faDownload} class="mr-2 h-4 w-4" />
					Descargar Audio
				</button>

				<button class="btn btn-secondary flex items-center px-4 py-1 h-8 rounded-md text-sm">
					<Fa icon={faFileAlt} class="mr-2 h-4 w-4" />
					Descargar Transcripción
				</button>
				{#if hasTwoOrMoreSelected}
					<button class="btn btn-secondary flex items-center px-4 py-1 h-8 rounded-md text-sm" on:click={openCombineModal}>
						<Fa icon={faObjectGroup} class="mr-2 h-4 w-4" />
						Combinar segmentos seleccionados
					</button>
				{/if}
			</div>
		</div>
		<!-- Transcription Table -->
		<div class="overflow-x-auto border border-gray-200 rounded-lg">
			<table class="min-w-full table-auto">
				<thead class="bg-gray-50">
				<tr>
					<th class="px-4 py-3 text-left text-gray-600 font-medium w-1/8">Interlocutor</th>
					<th class="px-4 py-3 text-left text-gray-600 font-medium w-5/6">Transcripción</th>
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
									class="flex items-center justify-center bg-gray-200 text-black rounded-md px-4 py-2 cursor-pointer"
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
								<button
									class="btn btn-sm relative group {segment.revised ? 'btn-success' : 'btn-secondary'}"
									on:click={() => handleAction('Revisar', index)}
								>
									<Fa icon={segment.revised ? faCheck : faEye} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                                        {segment.revised ? 'Marcar para revisar' : 'Validar transcripcion'}
                                    </span>
								</button>

								<button class="btn btn-sm btn-secondary relative group" on:click={() => handleAction('Revertir', index)}>
									<Fa icon={faUndo} class="h-5 w-5" />
									<span class="absolute z-50 left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                                        Revertir
                                    </span>
								</button>

								<button class="btn btn-sm btn-warning relative group" on:click={() => handleAction('Dividir', index)}>
									<Fa icon={faCut} class="h-5 w-5" />
									<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                                        Dividir
                                    </span>
								</button>

								<button class="btn btn-sm relative group {segment.selected ? 'btn-success' : 'btn-secondary'}" on:click={() => toggleSegmentSelection(index)}>
									<Fa icon={faCompressArrowsAlt} class="h-5 w-5" />
									<span class="absolute z-50 left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
                                        {segment.selected ? 'Deseleccionar' : 'Seleccionar'}
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

		<!-- Rename Interlocutors Modal -->
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
							class="btn btn-secondary w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 text-base font-medium text-black focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:ml-3 sm:w-auto sm:text-sm"
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

		<!-- Combine Segments Modal -->
		{#if showCombineModal}
			<div class="fixed inset-0 flex items-center justify-center z-50">
				<!-- Background overlay -->
				<div class="absolute inset-0 bg-black opacity-50"></div>

				<!-- Modal Content -->
				<div class="bg-white rounded-lg shadow-xl transform transition-all sm:max-w-lg sm:w-full z-50 p-6">
					<!-- Modal Title -->
					<h3 class="text-xl font-bold text-gray-900 mb-6">Combinar Segmentos Seleccionados</h3>

					<!-- Display Selected Segments -->
					<div class="space-y-4">
						{#each segmentsToCombine as segment}
							<div class="p-4 bg-gray-100 rounded-lg">
								<p class="text-sm text-gray-700">
									<strong>Interlocutor:</strong> {segment.speaker === 'SPEAKER_00' ? $speakerName1 : $speakerName2}
								</p>
								<p class="text-gray-800 mt-1">{segment.text}</p>
							</div>
						{/each}
					</div>

					<!-- Action buttons -->
					<div class="mt-8 flex justify-end space-x-4">
						<button
							type="button"
							class="btn btn-success px-6"
							on:click={combineSegments}
						>
							Combinar
						</button>
						<button
							type="button"
							class="btn btn-secondary px-6"
							on:click={closeCombineModal}
						>
							Cerrar
						</button>
					</div>
				</div>
			</div>
		{/if}

		<!-- Divide Segment Modal -->
		{#if showDivideModal}
			<div class="fixed inset-0 flex items-center justify-center z-50">
				<div class="absolute inset-0 bg-black opacity-50"></div>
				<div class="bg-white rounded-lg overflow-hidden shadow-xl transform transition-all sm:max-w-lg sm:w-full z-50 p-6">
					<!-- Modal Title -->
					<h3 class="text-xl font-bold text-gray-900 mb-4">Dividir segmento</h3>

					<!-- Custom Audio Player -->
					<div class="flex items-center bg-gray-400 rounded-full px-4 py-2 w-full max-w-md mb-4">
						<!-- Play/Pause Button -->
						<button class="flex items-center justify-center w-8 h-8 bg-gray-600 rounded-full text-white mr-3">
							<!-- Use SVG icon for play/pause (swap based on playback state) -->
							<svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
								<path d="M5 3.5v17l15-8.5-15-8.5z" /> <!-- Play Icon -->
								<!-- For pause icon, swap path to: <path d="M6 4h4v16h-4v-16zm8 0h4v16h-4v-16z"/> -->
							</svg>
						</button>

						<!-- Current Time Display -->
						<span class="text-white text-sm mr-3">0:00</span>

						<!-- Progress Bar -->
						<div class="relative flex-1 h-1 bg-gray-300 rounded-full mr-3">
							<div class="absolute top-0 left-0 h-full w-1/4 bg-gray-600 rounded-full"></div> <!-- Example Progress -->
							<div class="absolute top-0 right-0 h-full w-1 bg-white rounded-full"></div> <!-- Handle -->
						</div>

						<!-- Total Time Display -->
						<span class="text-white text-sm">0:00</span>
					</div>

					<!-- Split at current time button -->
					<button class="btn btn-default hover w-full mb-4">Separar en el segundo actual</button>

					<!-- Segments Display -->
					<div class="space-y-2">
						<!-- Segment 1 -->
						<div class="flex items-center justify-between">
							<span class="text-gray-700 font-medium">Segment 1: 0.00s - 0.89s</span>
							<select class="select select-bordered w-1/3">
								<option value="SPEAKER_00">SPEAKER_00</option>
								<option value="SPEAKER_01">SPEAKER_01</option>
							</select>
							<button class="btn btn-error btn-sm">Eliminar</button>
						</div>

						<!-- Segment 2 -->
						<div class="flex items-center justify-between">
							<span class="text-gray-700 font-medium">Segment 2: 0.80s - 0.89s</span>
							<select class="select select-bordered w-1/3">
								<option value="SPEAKER_00">SPEAKER_00</option>
								<option value="SPEAKER_01">SPEAKER_01</option>
							</select>
							<button class="btn btn-error btn-sm">Eliminar</button>
						</div>
					</div>

					<!-- Action buttons -->
					<div class="mt-6 flex justify-end space-x-4">
						<button
							type="button"
							class="btn btn-success"
							on:click={divideSegment}
						>
							Dividir
						</button>
						<button
							type="button"
							class="btn btn-secondary"
							on:click={closeDivideModal}
						>
							Cancelar
						</button>
					</div>
				</div>
			</div>
		{/if}
	</div>
</section>
