<script>
	import { faPhone, faMailBulk, faDirections } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';
</script>

<section class="px-6 sm:px-10 py-12 sm:py-16">
	<div class="container mx-auto max-w-6xl bg-white shadow-md rounded-lg p-6 sm:p-8">
		<!-- Main Heading -->
		<h1 class="text-3xl sm:text-5xl font-bold text-gray-800 mb-4 text-center">Contacto</h1>
		<p class="text-gray-600 text-center mb-8 sm:mb-10">Estamos aquí para ayudarte.</p>

		<div class="grid grid-cols-1 lg:grid-cols-2 gap-10">
			<!-- Contact Information -->
			<div class="space-y-6 sm:space-y-8">
				<h2 class="text-xl sm:text-2xl font-semibold text-gray-800">Contacta Phonetic</h2>
				<p class="text-gray-600">
					Llena el formulario o contáctanos directamente a través de este mail o teléfono.
				</p>

				<div class="space-y-4">
					<!-- Location -->
					<div class="flex items-center space-x-3">
						<Fa icon={faDirections} class="h-5 w-5 sm:h-6 sm:w-6 text-gray-500"/>
						<span class="text-gray-700">Dirección</span>
					</div>

					<!-- Email -->
					<div class="flex items-center space-x-3">
						<Fa icon={faMailBulk} class="h-5 w-5 sm:h-6 sm:w-6 text-gray-500"/>
						<span class="text-gray-700">ejemplo@phonetic.com</span>
					</div>

					<!-- Phone -->
					<div class="flex items-center space-x-3">
						<Fa icon={faPhone} class="h-5 w-5 sm:h-6 sm:w-6 text-gray-500"/>
						<span class="text-gray-700">+34 número de teléfono</span>
					</div>
				</div>
			</div>

			<!-- Contact Form -->
			<div>
				<form class="space-y-4 sm:space-y-6">
					<!-- Name Field -->
					<div>
						<label for="name" class="block text-sm font-medium text-gray-700">Nombre Completo:</label>
						<input
							type="text"
							id="name"
							placeholder="Nombre Completo"
							class="mt-2 block w-full px-3 py-2 sm:px-4 sm:py-3 border rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
						/>
					</div>

					<!-- Email Field -->
					<div>
						<label for="email" class="block text-sm font-medium text-gray-700">Correo Electrónico:</label>
						<input
							type="email"
							id="email"
							placeholder="Correo Electrónico"
							class="mt-2 block w-full px-3 py-2 sm:px-4 sm:py-3 border rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
						/>
					</div>

					<!-- Message Field -->
					<div>
						<label for="message" class="block text-sm font-medium text-gray-700">Tu mensaje aquí:</label>
						<textarea
							id="message"
							rows="5"
							placeholder="Tu mensaje aquí..."
							class="mt-2 block w-full px-3 py-2 sm:px-4 sm:py-3 border rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
						></textarea>
					</div>

					<!-- Submit Button -->
					<div>
						<button
							type="submit"
							class="w-full py-3 px-4 btn btn-primary text-white font-semibold rounded-lg shadow-md hover:bg-indigo-600 focus:outline-none focus:ring-2 focus:ring-indigo-500"
						>
							Enviar Mensaje
						</button>
					</div>
				</form>
			</div>
		</div>
	</div>
</section>
