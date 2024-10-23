<script>
	import {faUpload, faFileExport, faSort } from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';
</script>

<section>
	<div class="body flex-grow-1 px-16">
		<div class="mt-12">
			<div class="flex isolate">
				<!-- Dropdown Button -->
				<div class="mr-2 relative inline-flex items-center">
					<button class="flex items-center bg-gray-200 hover:bg-gray-300 text-gray-800 font-semibold py-2 px-4 rounded-full">
						<Fa icon={faSort} class="h-6 w-6 mr-1" />
						Newest first
					</button>
				</div>

				<!-- Search Input -->
				<div class="flex items-center bg-white py-2 px-4 rounded-[24px]">
          <span aria-hidden="true" class="inline-flex items-center justify-center" role="img">
            <svg fill="currentColor" width="20" height="20" viewBox="0 0 24 24">
              <path d="M9.5,3A6.5,6.5 0 0,1 16,9.5C16,11.11 15.41,12.59 14.44,13.73L14.71,14H15.5L20.5,19L19,20.5L14,15.5V14.71L13.73,14.44C12.59,15.41 11.11,16 9.5,16A6.5,6.5 0 0,1 3,9.5A6.5,6.5 0 0,1 9.5,3M9.5,5C7,5 5,7 5,9.5C5,12 7,14 9.5,14C12,14 14,12 14,9.5C14,7 12,5 9.5,5Z"></path>
            </svg>
          </span>
					<input
						placeholder="Search"
						class="ml-4 pr-4 pl-4 focus:outline-none"
					>
				</div>

				<!-- Spacer -->
				<div class="ml-auto"></div>

				<!-- Right Section Buttons -->
				<div class="flex space-x-2">
					<!-- Transcribe Button -->
					<div>
						<button class="flex items-center btn btn-secondary text-black font-semibold py-2 px-4 rounded-full hover:bg-base-200" aria-label="Transcribe">
							<span class="mr-3">Transcribe</span>
							<Fa icon={faFileExport} class="h-6 w-6"/>
						</button>
					</div>

					<!-- Create Folder Button -->
					<button class="flex items-center justify-center bg-gray-200 hover:bg-gray-300 p-2 rounded-full" type="button" aria-label="Create new folder">
						<Fa icon={faUpload} class="h-6 w-6"/>
					</button>

					<!-- Filter Options Button -->
					<button class="flex items-center bg-gray-200 hover:bg-gray-300 p-2 rounded-full" type="button" aria-label="Filter options">
            <span class="inline-flex" aria-hidden="true">
              <svg fill="currentColor" class="w-6 h-6" viewBox="0 0 24 24">
                <path d="M6,13H18V11H6M3,6V8H21V6M10,18H14V16H10V18Z"></path>
              </svg>
            </span>
					</button>
				</div>
			</div>

			<!-- Table Section -->
			<div class="mt-6 border border-gray-200 rounded-tl-[24px] rounded-tr-[24px]">
				<div class="relative overflow-x-auto">
					<!-- Table Header -->
					<table class="w-full">
						<thead class="bg-gray-50">
						<tr>
							<th class="p-4 text-left">
								<input type="checkbox" class="h-5 w-5 rounded-full text-blue-600">
							</th>
							<!-- Added text-right to align the text to the right for each header -->
							<th class="p-4 text-gray-600 font-semibold text-right">Name</th>
							<th class="p-4 text-gray-600 font-semibold text-right">Speakers</th>
							<th class="p-4 text-gray-600 font-semibold text-right">Duration</th>
							<th class="p-4 text-gray-600 font-semibold text-right">Created at</th>
						</tr>
						</thead>
						<tbody class="bg-white">
						<!-- No records found row -->
						<tr>
							<td colspan="5" class="p-4 text-left bg-gray-100 text-gray-600 hover:bg-zinc-200">
								No records found
							</td>
						</tr>
						</tbody>
					</table>


					<!-- Pagination -->
					<div class="flex items-center justify-between bg-white p-4 border-t border-gray-200 rounded-bl-[24px] rounded-br-[24px]">
						<p class="text-sm text-gray-600">1 - 10 of 0 items</p>
						<div class="flex items-center space-x-2">
							<button class="px-3 py-1 rounded-full border text-blue-500">1</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>
