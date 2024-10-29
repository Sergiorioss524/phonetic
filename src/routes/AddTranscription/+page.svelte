<script>
	let speakersCount = 1;

	// Increment and decrement functions for the counter
	function incrementSpeakers() {
		speakersCount = Math.min(speakersCount + 1, 5); // Set maximum value to 3
	}

	function decrementSpeakers() {
		speakersCount = Math.max(speakersCount - 1, 1); // Set minimum value to 1
	}
</script>

<section class="px-4 sm:px-8 md:px-10 py-8 sm:py-12 md:py-16">
	<div class="container mx-auto max-w-2xl md:max-w-4xl bg-white shadow-md rounded-lg p-6 sm:p-8">
		<h1 class="text-3xl sm:text-4xl font-bold text-gray-800 mb-4 sm:mb-6">Carga y transcribe audio</h1>

		<div class="mb-4 sm:mb-6">
			<label for="interlocutores" class="block text-sm font-medium text-gray-700 mb-2">Interlocutores:</label>
			<div class="flex items-center">
				<!-- Decrement Button -->
				<button
					type="button"
					class="px-4 py-2 bg-gray-200 text-gray-800 font-semibold rounded-l-lg hover:bg-gray-300"
					on:click={decrementSpeakers}
					aria-label="Decrease speakers count"
				>
					-
				</button>
				<!-- Display Count -->
				<span class="w-12 text-center px-4 py-2 border-t border-b border-gray-300">{speakersCount}</span>
				<!-- Increment Button -->
				<button
					type="button"
					class="px-4 py-2 bg-gray-200 text-gray-800 font-semibold rounded-r-lg hover:bg-gray-300"
					on:click={incrementSpeakers}
					aria-label="Increase speakers count"
				>
					+
				</button>
			</div>
		</div>

		<div class="mb-4 sm:mb-6">
			<label for="transcription-name" class="block text-sm font-medium text-gray-700">Nombre de la transcripción:</label>
			<input
				type="text"
				id="transcription-name"
				placeholder="Introduce el nombre"
				class="mt-2 block w-full px-3 py-2 border rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
			/>
		</div>

		<div class="mb-4 sm:mb-6">
			<label for="file-upload" class="block text-sm font-medium text-gray-700">Selecciona un archivo:</label>
			<div class="mt-2 flex flex-col items-center justify-center sm:flex-row px-6 pt-5 pb-6 border-2 border-gray-300 border-dashed rounded-lg">
				<input type="file" id="file-upload" class="hidden" />
				<label for="file-upload" class="cursor-pointer text-sm text-gray-500 hover:text-indigo-600">
					<span class="text-blue-500">Choose File</span> (No file selected)
				</label>
			</div>
		</div>

		<div class="mb-4 sm:mb-6">
			<a href="/Viewtranscription">
				<button
					class="w-full py-3 px-4 btn btn-secondary text-white font-semibold rounded-lg shadow-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
				>
					Carga y transcribe
				</button>
			</a>
		</div>
	</div>
</section>
