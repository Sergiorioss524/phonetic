<script lang="ts">
	import { writable } from 'svelte/store';
	import { faEye, faUndo, faCut, faCompressArrowsAlt, faTrashAlt, faCheck, faEllipsisV } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';
	import { faUserEdit, faDownload, faEdit, faFileAlt, faObjectGroup } from '@fortawesome/free-solid-svg-icons';

	interface Transcription {
		speaker: string;
		text: string;
		audioUrl: string;
		duration: string;
		selected?: boolean;
		revised?: boolean;
		showMenu?: boolean;
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

	// Function to toggle action menu visibility
	function toggleActionMenu(index: number) {
		transcriptionStore.update(transcriptions => {
			transcriptions.forEach((segment, i) => {
				if (i === index) {
					segment.showMenu = !segment.showMenu;
				} else {
					segment.showMenu = false;
				}
			});
			return transcriptions;
		});
	}
</script>

<section class="px-4 md:px-10 py-8 md:py-16">
	<div class="container mx-auto max-w-7xl bg-white shadow-md rounded-lg p-4 md:p-8">
		<!-- Header Section -->
		<h1 class="text-2xl md:text-4xl font-bold text-gray-800 mb-4 md:mb-6">Transcripción Completa</h1>
		<div class="flex flex-col md:flex-row justify-between gap-4 mb-6">
			<!-- Main Buttons -->
			<div class="flex flex-wrap gap-2">
				<button class="btn btn-secondary flex items-center px-3 md:px-4 py-2 rounded-md text-sm" on:click={openRenameModal}>
					<Fa icon={faUserEdit} class="mr-2 h-4 w-4" />
					Renombrar interlocutores
				</button>
				<button class="btn btn-secondary flex items-center px-3 md:px-4 py-2 rounded-md text-sm">
					<Fa icon={faDownload} class="mr-2 h-4 w-4" />
					Descargar Audio
				</button>
				<button class="btn btn-secondary flex items-center px-3 md:px-4 py-2 rounded-md text-sm">
					<Fa icon={faEdit} class="mr-2 h-4 w-4" />
					Renombrar transcripción
				</button>
				<button class="btn btn-secondary flex items-center px-3 md:px-4 py-2 rounded-md text-sm">
					<Fa icon={faFileAlt} class="mr-2 h-4 w-4" />
					Descargar Transcripción
				</button>
				{#if hasTwoOrMoreSelected}
					<button class="btn btn-secondary flex items-center px-3 md:px-4 py-2 rounded-md text-sm" on:click={openCombineModal}>
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
					<th class="px-2 md:px-4 py-3 text-left text-gray-600 font-medium w-1/6">Interlocutor</th>
					<th class="px-2 md:px-4 py-3 text-left text-gray-600 font-medium w-5/6">Transcripción</th>
					<th class="px-2 md:px-2 py-3 text-left text-gray-600 font-medium w-1/6">Acciones</th>
				</tr>
				</thead>
				<tbody class="bg-white divide-y divide-gray-200">
				{#each $transcriptionStore as segment, index}
					<tr class="{segment.selected ? 'bg-gray-200' : ''}">
						<td class="px-2 md:px-4 py-4 text-sm font-medium text-gray-800">
							<!-- Dropdown to select speaker -->
							<select on:change={(e) => handleSelectChange(index, e)} class="border rounded-lg px-2 py-1 w-full">
								<option value="SPEAKER_00" selected={segment.speaker === 'SPEAKER_00'}>{$speakerName1}</option>
								<option value="SPEAKER_01" selected={segment.speaker === 'SPEAKER_01'}>{$speakerName2}</option>
							</select>
						</td>
						<td class="px-2 md:px-4 py-4">
							<div class="flex items-center space-x-2 md:space-x-3">
								<!-- Custom Play Button with Duration -->
								<button
									class="flex items-center justify-center bg-gray-200 text-black rounded-full px-2 md:px-4 py-2 cursor-pointer"
									style="font-family: 'Courier New', monospace; font-size: 1rem;"
								>
									<span class="mr-2 text-gray-600 text-lg">▶</span>
									<span class="text-black">00:00</span>
								</button>

								<!-- Text Area for Transcription -->
								<textarea
									class="w-full px-2 md:px-3 py-2 border rounded-lg resize-none focus:outline-none focus:ring-2 focus:ring-indigo-500"
									bind:value={segment.text}
									rows="1"
									style="overflow:hidden;"
									on:input={autoResize}
								></textarea>
							</div>
						</td>
						<td class="px-2 md:px-7 py-4">
							<!-- Desktop Action Buttons -->
							<div class="hidden md:flex flex-wrap md:flex-nowrap space-x-1 md:space-x-2 justify-center">
								<!-- Tooltip-enabled buttons -->
								<button
									class="btn btn-sm relative group {segment.revised ? 'btn-success' : 'btn-secondary'}"
									on:click={() => handleAction('Revisar', index)}
								>
									<Fa icon={segment.revised ? faCheck : faEye} class="h-5 w-5" />
								</button>

								<button class="btn btn-sm btn-secondary relative group" on:click={() => handleAction('Revertir', index)}>
									<Fa icon={faUndo} class="h-5 w-5" />
								</button>

								<button class="btn btn-sm btn-warning relative group" on:click={() => handleAction('Dividir', index)}>
									<Fa icon={faCut} class="h-5 w-5" />
								</button>

								<button class="btn btn-sm relative group {segment.selected ? 'btn-success' : 'btn-secondary'}" on:click={() => toggleSegmentSelection(index)}>
									<Fa icon={faCompressArrowsAlt} class="h-5 w-5" />
								</button>

								<button class="btn btn-sm btn-danger relative group" on:click={() => handleAction('Eliminar', index)}>
									<Fa icon={faTrashAlt} class="h-5 w-5" />
								</button>
							</div>
							<!-- Mobile Action Menu -->
							<div class="md:hidden relative">
								<button class="btn btn-sm btn-secondary" on:click={() => toggleActionMenu(index)}>
									<Fa icon={faEllipsisV} class="h-5 w-5" />
								</button>
								{#if segment.showMenu}
									<div class="absolute right-0 mt-2 w-48 bg-white border border-gray-200 rounded-md shadow-lg z-10">
										<button class="block w-full text-left px-4 py-2 text-sm text-gray-700 hover:bg-gray-100" on:click={() => handleAction('Revisar', index)}>
											{#if segment.revised}
												<Fa icon={faCheck} class="inline-block mr-2" /> Marcar para revisar
											{:else}
												<Fa icon={faEye} class="inline-block mr-2" /> Validar transcripción
											{/if}
										</button>
										<button class="block w-full text-left px-4 py-2 text-sm text-gray-700 hover:bg-gray-100" on:click={() => handleAction('Revertir', index)}>
											<Fa icon={faUndo} class="inline-block mr-2" /> Revertir
										</button>
										<button class="block w-full text-left px-4 py-2 text-sm text-gray-700 hover:bg-gray-100" on:click={() => handleAction('Dividir', index)}>
											<Fa icon={faCut} class="inline-block mr-2" /> Dividir
										</button>
										<button class="block w-full text-left px-4 py-2 text-sm text-gray-700 hover:bg-gray-100" on:click={() => toggleSegmentSelection(index)}>
											<Fa icon={faCompressArrowsAlt} class="inline-block mr-2" /> {segment.selected ? 'Deseleccionar' : 'Seleccionar'}
										</button>
										<button class="block w-full text-left px-4 py-2 text-sm text-red-600 hover:bg-gray-100" on:click={() => handleAction('Eliminar', index)}>
											<Fa icon={faTrashAlt} class="inline-block mr-2" /> Eliminar
										</button>
									</div>
								{/if}
							</div>
						</td>
					</tr>
				{/each}
				</tbody>
			</table>
		</div>

		<!-- Rename Interlocutors Modal -->
		{#if showRenameModal}
			<div class="fixed inset-0 flex items-center justify-center z-50 px-4">
				<div class="absolute inset-0 bg-black opacity-50"></div>
				<div class="bg-white rounded-lg overflow-hidden shadow-xl transform transition-all sm:max-w-lg w-full sm:w-full z-50">
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
							class="btn btn-secondary w-full sm:w-auto inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 text-base font-medium text-black focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:ml-3 sm:text-sm"
							on:click={saveSpeakerNames}
						>
							Guardar
						</button>
						<button
							type="button"
							class="mt-3 w-full sm:w-auto inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:text-sm"
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
			<div class="fixed inset-0 flex items-center justify-center z-50 px-4">
				<div class="absolute inset-0 bg-black opacity-50"></div>
				<div class="bg-white rounded-lg shadow-xl transform transition-all sm:max-w-lg w-full sm:w-full z-50 p-4 sm:p-6">
					<h3 class="text-lg md:text-xl font-bold text-gray-900 mb-4">Combinar Segmentos Seleccionados</h3>
					<div class="space-y-2">
						{#each segmentsToCombine as segment}
							<div class="p-2 sm:p-4 bg-gray-100 rounded-lg">
								<p class="text-sm text-gray-700">
									<strong>Interlocutor:</strong> {segment.speaker === 'SPEAKER_00' ? $speakerName1 : $speakerName2}
								</p>
								<p class="text-gray-800 mt-1">{segment.text}</p>
							</div>
						{/each}
					</div>
					<div class="mt-6 flex flex-col sm:flex-row-reverse gap-2 sm:gap-4">
						<button class="btn btn-success w-full sm:w-auto px-4 py-2" on:click={combineSegments}>Combinar</button>
						<button class="btn btn-secondary w-full sm:w-auto px-4 py-2" on:click={closeCombineModal}>Cerrar</button>
					</div>
				</div>
			</div>
		{/if}

		<!-- Divide Segment Modal -->
		{#if showDivideModal}
			<div class="fixed inset-0 flex items-center justify-center z-50 px-4">
				<div class="absolute inset-0 bg-black opacity-50"></div>
				<div class="bg-white rounded-lg shadow-xl transform transition-all sm:max-w-lg w-full sm:w-full z-50 p-4 sm:p-6">
					<h3 class="text-lg md:text-xl font-bold text-gray-900 mb-4">Dividir segmento</h3>

					<!-- Custom Play Button with Duration -->
					<button
						class="flex items-center justify-center bg-gray-200 text-black rounded-full px-4 py-2 cursor-pointer mb-4"
						style="font-family: 'Courier New', monospace; font-size: 1rem;"
					>
						<!-- Play Icon and Time Text -->
						<span class="mr-2 text-gray-600 text-lg">▶</span>
						<span class="text-black">00:00</span>
					</button>

					<!-- Speaker Selection Dropdown -->
					<div class="mb-4">
						<label class="block text-gray-700 text-sm font-bold mb-2" for="divideSpeaker">
							Asignar nuevo segmento a:
						</label>
						<select id="divideSpeaker" bind:value={selectedSpeakerForDivide} on:change={handleSpeakerChangeForDivide} class="border rounded-lg px-2 py-1 w-full">
							<option value="SPEAKER_00">{$speakerName1}</option>
							<option value="SPEAKER_01">{$speakerName2}</option>
						</select>
					</div>

					<!-- Textarea for editing and specifying where to divide -->
					<textarea
						class="w-full px-3 py-2 border rounded-lg resize-none focus:outline-none focus:ring-2 focus:ring-indigo-500"
						bind:value={divideText}
						rows="5"
						style="overflow:auto;"
					></textarea>
					<p class="text-sm text-gray-600 mt-2">
						Seleccione el punto donde desea dividir el segmento.
					</p>

					<div class="mt-6 flex flex-col sm:flex-row-reverse gap-2 sm:gap-4">
						<button class="btn btn-success w-full sm:w-auto px-4 py-2" on:click={divideSegment}>Dividir</button>
						<button class="btn btn-secondary w-full sm:w-auto px-4 py-2" on:click={closeDivideModal}>Cancelar</button>
					</div>
				</div>
			</div>
		{/if}

	</div>
</section>
