
<script>
	import Fa from 'svelte-fa';
	import {
		faEdit,
		faTrashAlt,
		faDownload,
		faFileAlt,
	} from '@fortawesome/free-solid-svg-icons';
</script>


<div class="overflow-x-auto px-24 mb-20">
	<table class="w-full table-auto border border-gray-200 rounded-[24px] overflow-hidden">
		<!-- Table Header -->
		<thead class="bg-white rounded-t-[24px]">
		<tr>
			<!-- Checkbox Header -->
			<th class="p-4 text-left rounded-tl-[24px]">
				<input type="checkbox" class="h-5 w-5 rounded-full border-gray-300 text-blue-600 focus:ring-blue-500">
			</th>
			<th class="p-4 text-gray-600 font-semibold text-left">Name</th>
			<th class="p-4 text-gray-600 font-semibold text-center">Creator</th>
			<th class="p-4 text-gray-600 font-semibold text-center">Created at</th>
			<th class="p-4 text-gray-600 font-semibold text-center">Transcription</th>
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
			<td class="p-4 text-center">John Doe</td>
			<td class="p-4 text-center">2023-10-31</td>
			<td class="p-4 text-center">Completed</td>
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
			<td class="p-4 text-center">Jane Smith</td>
			<td class="p-4 text-center">2023-10-30</td>
			<td class="p-4 text-center">In Progress</td>
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
