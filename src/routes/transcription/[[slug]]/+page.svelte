<script lang="ts">
	import {faSort } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';
	import { writable } from 'svelte/store';
	import Teamtranscript from '../Teamtranscript.svelte';

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
			<div class="flex flex-col sm:flex-row isolate space-y-4 sm:space-y-0">
				<!-- Dropdown Button -->
				<div class="sm:mr-2 relative inline-flex items-center z-[200]">
					<div class="dropdown dropdown-right z-[200]">
						<div tabindex="0" role="button" class="btn btn-secondary rounded-full m-1 font-semibold py-2 px-4 w-full sm:w-auto">
							<Fa icon={faSort} class="h-6 w-6 mr-1" /> Select Order
						</div>
						<ul class="dropdown-content menu bg-base-100 rounded-box z-[300] w-52 p-2 shadow">
							<li><button on:click={() => changeSortOrder('alphabetical')}>Alphabetical</button></li>
							<li><button on:click={() => changeSortOrder('createdAt')}>Created At</button></li>
						</ul>
					</div>
				</div>

				<!-- Search Input (Ensuring visibility and proper styling) -->
				<div class="flex items-center bg-white py-2 px-4 rounded-[24px] w-full sm:w-auto">
          <span aria-hidden="true" class="inline-flex items-center justify-center" role="img">
            <svg fill="currentColor" width="20" height="20" viewBox="0 0 24 24">
              <path d="M9.5,3A6.5,6.5 0 0,1 16,9.5C16,11.11 15.41,12.59 14.44,13.73L14.71,14H15.5L20.5,19L19,20.5L14,15.5V14.71L13.73,14.44C12.59,15.41 11.11,16 9.5,16A6.5,6.5 0 0,1 3,9.5A6.5,6.5 0 0,1 9.5,3M9.5,5C7,5 5,7 5,9.5C5,12 7,14 9.5,14C12,14 14,12 14,9.5C14,7 12,5 9.5,5Z"></path>
            </svg>
          </span>
					<input placeholder="Search" class="ml-4 pr-4 pl-4 focus:outline-none w-full sm:w-auto">
				</div>

				<!-- Spacer -->
				<div class="ml-auto"></div>

				<!-- Right Section Buttons -->
				<div class="flex space-x-2 justify-between sm:justify-start">
					<!-- Transcriptions Button -->
					<div>
						<a href="/AddTranscription">
						<button class="btn btn-primary flex items-center text-black font-semibold py-4 px-8 rounded-full sm:w-auto">
							Transcriptions
						</button>
						</a>
					</div>
				</div>
			</div>

			<!-- Table Section -->
			<div class="mt-6 border border-gray-200 rounded-tl-[24px] rounded-tr-[24px] overflow-x-auto relative z-[0]">
				<div class="relative overflow-x-auto">
					<!-- Table Header -->
					<table class="w-full table-auto">
						<thead class="bg-gray-50">
						<tr>
							<!-- Rounded Checkbox Header -->
							<th class="p-4 text-left">
								<input type="checkbox" class="h-5 w-5 rounded-full border-gray-300 text-blue-600 focus:ring-blue-500">
							</th>
							<!-- Headers with proper alignment and spacing -->
							<th class="p-4 text-gray-600 font-semibold text-left">Name</th>
							<th class="p-4 text-gray-600 font-semibold text-center">Speakers</th>
							<th class="p-4 text-gray-600 font-semibold text-center">Duration</th>
							<th class="p-4 text-gray-600 font-semibold text-center">Created at</th>
							<th class="p-4 text-gray-600 font-semibold text-center">Transcription</th>
						</tr>
						</thead>
						<tbody class="bg-white">
						<!-- Show files if any -->
						{#each $filesData as file, index}
							<tr class="hover:bg-gray-50">
								<td class="p-4 text-left">
									<input type="checkbox" class="h-5 w-5 rounded-full border-gray-300 text-blue-600 focus:ring-blue-500" on:change={() => handleCheckboxChange(index)}>
								</td>
								<td class="p-4 text-left">{file.name}</td>
								<!-- Numeric Counter for Speakers -->
								<td class="p-4 text-center">
									<div class="flex items-center justify-center space-x-2">
										<button class="px-2 py-1 bg-gray-200 hover:bg-gray-300 rounded" on:click={() => updateSpeakers(index, -1)}>-</button>
										<input
											type="text"
											value={file.speakers}
											readonly
											class="w-12 text-center border border-gray-300 rounded p-1 focus:outline-none"
										/>
										<button class="px-2 py-1 bg-gray-200 hover:bg-gray-300 rounded" on:click={() => updateSpeakers(index, 1)}>+</button>
									</div>
								</td>
								<td class="p-4 text-center">{file.duration}</td>
								<td class="p-4 text-center">{file.createdAt}</td>
								<td class="p-4 text-center">
									<button
										class={`px-2 py-1 rounded ${file.transcription ? 'bg-green-500' : 'bg-red-500'} text-white`}
										on:click={() => toggleTranscription(index)}
									>
										{file.transcription ? 'Yes' : 'No'}
									</button>
								</td>
							</tr>
						{/each}

						<!-- No records found row -->
						{#if $filesData.length === 0}
							<tr>
								<td colspan="6" class="p-4 text-left bg-gray-100 text-gray-600 hover:bg-zinc-200">
									No records found
								</td>
							</tr>
						{/if}
						</tbody>
					</table>

					<!-- Pagination (Static example) -->
					<div class="flex items-center justify-between bg-white p-4 border-t border-gray-200 rounded-bl-[24px] rounded-br-[24px]">
						<p class="text-xs sm:text-sm text-gray-600">1 - 10 of {$filesData.length} items</p>
						<div class="flex items-center space-x-2">
							<button class="px-3 py-1 rounded-full border text-blue-500">1</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>

<section class="mt-24">
	<!-- Stylish Heading -->
	<h2 class="text-4xl font-bold text-gray-800 mb-12 text-left ml-24">
		Transcripciones del equipo
	</h2>
	<Teamtranscript />
</section>
