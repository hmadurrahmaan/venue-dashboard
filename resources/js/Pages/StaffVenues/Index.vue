<template>
	<AppLayout title="Venues">

		<template #header>
			<h2 class="flex justify-between text-xl font-semibold leading-tight text-gray-800">
				<p>
                    Venue
					<i class="fa-solid fa-user-group"></i>
				</p>
			</h2>
		</template>

		<div class="py-12">

			<div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
				<div class="overflow-hidden bg-white shadow-xl sm:rounded-lg">

					<div class="flex justify-end mt-3">
						<div class="mb-3 xl:w-96">
							<div class="relative flex items-stretch w-4/5 mb-3 input-group">

								<input id="search" type='text' v-model="term" @keyup="search"
									class="outline-none focus:ring-0 rounded-r-none form-control relative min-w-0 block w-full px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0"
									placeholder="Search...">

								<button
									class="rounded-l-none btn px-6 py-2.5 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700  focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out flex items-center"
									type="button" id="button-addon2">
									<svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="search"
										class="w-4" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
										<path fill="currentColor"
											d="M505 442.7L405.3 343c-4.5-4.5-10.6-7-17-7H372c27.6-35.3 44-79.7 44-128C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c48.3 0 92.7-16.4 128-44v16.3c0 6.4 2.5 12.5 7 17l99.7 99.7c9.4 9.4 24.6 9.4 33.9 0l28.3-28.3c9.4-9.4 9.4-24.6.1-34zM208 336c-70.7 0-128-57.2-128-128 0-70.7 57.2-128 128-128 70.7 0 128 57.2 128 128 0 70.7-57.2 128-128 128z">
										</path>
									</svg>
								</button>
							</div>
						</div>
					</div>

					<table class="min-w-full divide-y divide-gray-200">
						<thead class="bg-gray-50">
							<tr>
								<th scope="col"
									class="px-6 py-3 text-xs font-medium tracking-wider text-center text-gray-500 uppercase">
									ID
								</th>
								<th scope="col"
									class="px-6 py-3 text-xs font-medium tracking-wider text-center text-gray-500 uppercase">
									Name
								</th>
								<th scope="col"
									class="px-6 py-3 text-xs font-medium tracking-wider text-center text-gray-500 uppercase">
									Email
								</th>
								<th scope="col"
									class="px-6 py-3 text-xs font-medium tracking-wider text-center text-gray-500 uppercase">
									Created at
								</th>
								<th scope="col" class="relative px-6 py-3">
									<span class="sr-only">Actions</span>
								</th>
							</tr>
						</thead>
						<tbody class="bg-white divide-y divide-gray-200">
							<tr v-for="venue in venues.data" :key="venue.id">
								<td class="px-6 py-4 whitespace-nowrap">
									<div class="text-sm text-center text-gray-900">
										{{ venue.venues.id }}
									</div>
								</td>
								<td class="px-6 py-4 whitespace-nowrap">
									<div class="flex items-center justify-center">

										<div class="ml-4">
											<div class="text-sm font-medium text-gray-900">
                                                {{ venue.venues.name }}
											</div>
										</div>
									</div>
								</td>
								<td class="px-6 py-4 whitespace-nowrap">
									<div class="text-sm text-center text-gray-900">
										<a :href="'mailto:' + venue.venues.email_address">{{ venue.venues.email_address }}</a>
									</div>
								</td>
								<td class="px-6 py-4 text-center whitespace-nowrap">
									<span
										class="inline-flex px-2 text-xs font-semibold leading-5 text-green-800 bg-green-100 rounded-full">
										{{ venue.venues.formatted_created_at }}
									</span>
								</td>
								<td class="px-6 py-4 text-sm font-medium text-right whitespace-nowrap">

									<inertia-link :href="`/venues/${venue.venues.id}/edit`"
										class="float-left px-4 py-2 text-green-400 duration-100 rounded hover:text-green-600">
										<svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none"
											viewBox="0 0 24 24" stroke="currentColor">
											<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
												d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
										</svg></inertia-link>
                                    <inertia-link :href="`/venues/assign/${venue.venues.id}`"
                                                  class="float-left px-4 py-2 text-green-400 duration-100 rounded hover:text-green-600">
                                        <i class="fa fa-eye"></i>
                                    </inertia-link>
								</td>
							</tr>
						</tbody>
					</table>
					<Pagination class="mt-6" :links="venues.links" />
				</div>
			</div>
		</div>
	</AppLayout>
</template>

<script>
import Pagination from '@/Components/Pagination.vue';
import AppLayout from '@/Layouts/AppLayout.vue';
export default {
	components: {
		AppLayout,
		Pagination
	},
	props: {
        venues: Object
	},
	data() {
		return {
			selectedVenues: [],
			term: this.$page.term || '',
		}
	},
	methods: {
		confirmAction(message, callback) {
			if (confirm(message)) {
				callback();
			}
		},
		deleteVenue: function (venue) {
			this.confirmAction('Are you sure you want to delete venue?', function () {
				this.$inertia.delete(route('venues.destroy', venue.id));
			}.bind(this));
		},
		search() {
			this.$inertia.replace(this.route('venues.index', { term: this.term }))
		},
		async deleteSelectedVenues() {
			if (this.selectedVenues.length === 0) {
				return;
			}

			const confirmed = confirm(`Are you sure you want to delete ${this.selectedVenues.length} venues(s)?`);
			if (!confirmed) {
				return;
			}

			try {
				await this.$inertia.post('/venues/delete', { ids: this.selectedVenues });
				this.selectedVenues = [];
			} catch (error) {
				console.error(error);
				alert('An error occurred while deleting the selected users.');
			}
		},
	}
}
</script>
