<script lang="ts">
	import {faSort } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';
	import { writable } from 'svelte/store';
	import Teamtranscript from '../Teamtranscript.svelte';
	import {
		faEdit,
		faTrashAlt,
		faDownload,
		faFileAlt,
	} from '@fortawesome/free-solid-svg-icons';

	// Define the interface for file data
	interface FileData {
		name: string;
		speakers: number;
		duration: string;
		createdAt: string;
		transcription: boolean;
		selected: boolean;
	}

	// Store for file data with proper type
	const filesData = writable<FileData[]>([]);
	const selectedFileIndex = writable<number | null>(null);

	// Store for sort order
	const sortOrder = writable<string>('alphabetical');

	// Function to handle file upload (commented out)
	/*
	function handleFileUpload(event: Event) {
		const input = event.target as HTMLInputElement;
		if (input.files && input.files.length > 0) {
			const file = input.files[0];
			const audio = new Audio(URL.createObjectURL(file));

			// Extract file metadata when loaded
			audio.onloadedmetadata = () => {
				const fileData: FileData = {
					name: file.name,
					speakers: 1,
					duration: formatDuration(audio.duration),
					createdAt: new Date().toLocaleString(),
					transcription: false,
					selected: false,
				};
				filesData.update((files) => [...files, fileData]);
			};
		}
	}
	*/

	// Helper function to format duration in mm:ss format
	function formatDuration(seconds: number): string {
		const minutes = Math.floor(seconds / 60);
		const remainingSeconds = Math.floor(seconds % 60);
		return `${minutes}:${remainingSeconds.toString().padStart(2, '0')}`;
	}

	// Function to toggle transcription status
	function toggleTranscription(index: number) {
		filesData.update((files) => {
			files[index].transcription = !files[index].transcription;
			return [...files];
		});
	}

	// Function to handle speaker count increment/decrement
	function updateSpeakers(index: number, delta: number) {
		filesData.update((files) => {
			const updatedCount = Math.max(0, files[index].speakers + delta);
			files[index].speakers = updatedCount;
			return [...files];
		});
	}

	// Function to handle checkbox change
	function handleCheckboxChange(index: number) {
		filesData.update((files) => {
			files[index].selected = !files[index].selected;
			return [...files];
		});

		selectedFileIndex.set(index);
	}

	// Function to handle sorting
	function changeSortOrder(order: string) {
		sortOrder.set(order);
		filesData.update((files) => {
			if (order === 'alphabetical') {
				return [...files].sort((a, b) => a.name.localeCompare(b.name));
			} else if (order === 'createdAt') {
				return [...files].sort((a, b) => new Date(a.createdAt).getTime() - new Date(b.createdAt).getTime());
			}
			return files;
		});
	}

	// Computed value to determine the button text based on the selected file
	$: selectedIndex = $selectedFileIndex;
	$: selectedFile = selectedIndex !== null ? $filesData[selectedIndex] : null;
	$: showViewTranscription = selectedFile && selectedFile.selected && selectedFile.transcription;
	$: showTranscript = selectedFile && selectedFile.selected && !selectedFile.transcription;
</script>

<section class="px-10">
	<div class="body flex-grow-1 px-8 sm:px-8 md:px-16 relative z-[10]">
		<div class="mt-8 sm:mt-12">
			<div class="flex flex-col sm:flex-row items-center space-y-4 sm:space-y-0">
				<!-- Dropdown Button -->
				<div class="sm:mr-2 inline-flex items-center">
					<div class="dropdown dropdown-right">
						<button tabindex="0"  class="btn btn-secondary rounded-md h-12 font-semibold px-4 w-full sm:w-auto flex items-center">
							<Fa icon={faSort} class="h-6 w-6 mr-1" /> Select Order
						</button>
						<ul class="dropdown-content menu bg-base-100 rounded-box w-52 p-2 shadow">
							<li><button on:click={() => changeSortOrder('alphabetical')}>Alphabetical</button></li>
							<li><button on:click={() => changeSortOrder('createdAt')}>Created At</button></li>
						</ul>
					</div>
				</div>

				<!-- Search Input -->
				<div class="flex items-center bg-white h-12 px-4 rounded-md w-full sm:w-auto">
        <span aria-hidden="true" class="inline-flex items-center justify-center" role="img">
            <svg fill="currentColor" width="20" height="20" viewBox="0 0 24 24">
                <path d="M9.5,3A6.5,6.5 0 0,1 16,9.5C16,11.11 15.41,12.59 14.44,13.73L14.71,14H15.5L20.5,19L19,20.5L14,15.5V14.71L13.73,14.44C12.59,15.41 11.11,16 9.5,16A6.5,6.5 0 0,1 3,9.5A6.5,6.5 0 0,1 9.5,3M9.5,5C7,5 5,7 5,9.5C5,12 7,14 9.5,14C12,14 14,12 14,9.5C14,7 12,5 9.5,5Z"></path>
            </svg>
        </span>
					<input placeholder="Search" class="ml-2 pr-4 pl-4 focus:outline-none w-full sm:w-auto text-sm">
				</div>

				<!-- Spacer to push the last button to the right -->
				<div class="ml-auto"></div>

				<!-- Transcriptions Button -->
				<div>
					<a href="/AddTranscription">
						<button class="btn btn-primary h-12 px-8 text-black font-semibold rounded-md flex items-center">
							Transcriptions
						</button>
					</a>
				</div>
			</div>

			<!-- Table Section -->
			<div class="mt-6 border border-gray-200 rounded-tl-[24px] rounded-tr-[24px] overflow-x-auto relative z-[0]">
				<div class="relative overflow-x-auto">
					<!-- Table Header -->
					<table class="w-full table-auto border border-gray-200 rounded-[24px] overflow-hidden">
						<!-- Table Header -->
						<thead class="bg-white rounded-t-[24px]">
						<tr>
							<!-- Checkbox Header -->
							<th class="p-4 text-left rounded-tl-[24px]">
								<input type="checkbox" class="h-5 w-5 rounded-full border-gray-300 text-blue-600 focus:ring-blue-500">
							</th>
							<th class="p-4 text-gray-600 font-semibold text-left">Name</th>
							<th class="p-4 text-gray-600 font-semibold text-center">Duration</th>
							<th class="p-4 text-gray-600 font-semibold text-center">Created at</th>
							<th class="p-4 text-gray-600 font-semibold text-center rounded-tr-[24px]">Actions</th>
						</tr>
						</thead>
						<tbody class="bg-white">
						<!-- Row 1 -->
						<tr class="hover:bg-gray-200 bg-gray-100">
							<td class="p-4 text-left">
								<input type="checkbox" class="h-5 w-5 rounded-full border-gray-300 text-blue-600 focus:ring-blue-500">
							</td>
							<td class="p-4 text-left">Cy Ganderton</td>
							<td class="p-4 text-center">12:20</td>
							<td class="p-4 text-center">2023-10-31</td>
							<td class="px-7 py-4">
								<div class="flex space-x-2 justify-center">
									<!-- Tooltip-enabled buttons with consistent styling -->

									<!-- View Button -->
									<button class="btn btn-sm btn-secondary relative group">
										<Fa icon={faEdit} class="h-5 w-5" />
										<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
				Editar
			</span>
									</button>

									<!-- Revert Button -->
									<button class="btn btn-sm btn-secondary relative group">
										<Fa icon={faTrashAlt} class="h-5 w-5" />
										<span class="absolute z-50 left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
				Eliminar
			</span>
									</button>

									<!-- Divide Button -->
									<button class="btn btn-sm btn-warning relative group">
										<Fa icon={faDownload} class="h-5 w-5" />
										<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
				Descargar Audio
			</span>
									</button>

									<!-- Select/Deselect Button -->
									<button class="btn btn-sm btn-secondary relative group">
										<Fa icon={faFileAlt} class="h-5 w-5" />
										<span class="absolute z-50 left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
				Descargar Transcripcion
			</span>
									</button>
								</div>
							</td>

						</tr>

						<!-- Row 2 -->
						<tr class="hover:bg-gray-200 bg-gray-100">
							<td class="p-4 text-left">
								<input type="checkbox" class="h-5 w-5 rounded-md border-gray-300 text-blue-600 focus:ring-blue-500">
							</td>
							<td class="p-4 text-left">Hart Hagerty</td>
							<td class="p-4 text-center">17:30</td>
							<td class="p-4 text-center">2023-10-30</td>
							<td class="px-7 py-4">
								<div class="flex space-x-2 justify-center">
									<!-- Tooltip-enabled buttons with consistent styling -->

									<!-- View Button -->
									<button class="btn btn-sm btn-secondary relative group">
										<Fa icon={faEdit} class="h-5 w-5" />
										<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
				Editar
			</span>
									</button>

									<!-- Revert Button -->
									<button class="btn btn-sm btn-secondary relative group">
										<Fa icon={faTrashAlt} class="h-5 w-5" />
										<span class="absolute z-50 left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
				Eliminar
			</span>
									</button>

									<!-- Divide Button -->
									<button class="btn btn-sm btn-warning relative group">
										<Fa icon={faDownload} class="h-5 w-5" />
										<span class="absolute left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
				Descargar Audio
			</span>
									</button>

									<!-- Select/Deselect Button -->
									<button class="btn btn-sm btn-secondary relative group">
										<Fa icon={faFileAlt} class="h-5 w-5" />
										<span class="absolute z-50 left-0 transform -translate-y-full bg-black text-white text-xs rounded-md px-2 py-1 opacity-0 group-hover:opacity-100">
				Descargar Transcripcion
			</span>
									</button>
								</div>
							</td>
						</tr>

						</tbody>
						<tfoot>
						<tr>
							<td colspan="6">
								<div class="flex items-center justify-between bg-white p-4 border-t border-gray-200 rounded-bl-[24px] rounded-br-[24px]">
									<p class="text-xs sm:text-sm text-gray-600">1 - 10 of 0 items</p>
									<div class="flex items-center space-x-2">
										<button class="px-3 py-1 rounded-full border text-blue-500">1</button>
										<!-- Additional page buttons as needed -->
									</div>
								</div>
							</td>
						</tr>
						</tfoot>
					</table>
				</div>
			</div>
		</div>
	</div>
</section>

<section class="mt-24">
	<h2 class="text-4xl font-bold text-gray-800 mb-12 text-left ml-24">
		Transcripciones del equipo
	</h2>
	<Teamtranscript />
</section>
