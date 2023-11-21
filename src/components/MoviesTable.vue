<template>
	<div>
		<div class="filtered-results-count">
			<p>Total number of results found: <strong>{{ totalFilteredMoviesCount }}</strong></p>
		</div>
		<table class="table table-hover table-striped table-bordered">
			<thead>
				<tr class="text-center">
					<th>#</th>
					<th>Title</th>
					<th>Production Year</th>
					<th>Cast</th>
					<th>Genres</th>
				</tr>
			</thead>
			<tbody>
				<tr class="text-center" v-for="(movie, index) in displayedMovies" :key="movie.title">
					<td>{{ index + 1 }}</td>
					<td>{{ movie.title }}</td>
					<td>{{ movie.year }}</td>
					<td>{{ formatList(movie.cast) }}</td>
					<td>{{ formatList(movie.genres) }}</td>
				</tr>
			</tbody>
		</table>
		<button @click="showMore" v-show="!noMoreResults" class="btn btn-info col-sm-12 mb-2 text-center">Show More</button>
		<button @click="showLess" v-show="someResults" class="btn btn-info col-sm-12 mb-4 text-center">Show Less</button>
	</div>
</template>

<script>
import _ from 'lodash';

export default {
	name: 'MoviesTable',
	props: {
		allMovies: Array,
		searchCriteria: Object,
	},
	data() {
		return {
			displayedMovies: [],
			allFilteredMovies: [],
			itemsPerPage: 10,
			currentPage: 1
		};
	},
	created() {
		this.displayedMovies = this.allMovies.slice(0, this.itemsPerPage);
	},
	watch: {
		filteredMovies(newFilteredMovies) {
			this.displayedMovies = newFilteredMovies;
			this.$emit('filtered', newFilteredMovies);
		},
		searchCriteria: {
			deep: true,
			handler() {
				this.currentPage = 1;
			}
		}
	},
	computed: {
		totalFilteredMoviesCount() {
			return this.allFilteredMovies.length;
		},
		filteredMovies() {
			this.applyFilters();
			return this.allFilteredMovies.slice(0, this.itemsPerPage * this.currentPage);
		},
		noMoreResults() {
			return this.allFilteredMovies.length <= this.itemsPerPage * this.currentPage;
		},
		someResults() {
			return this.currentPage > 1;
		}
	},
	methods: {
		applyFilters() {
			const castQueries = this.searchCriteria.cast.split(',').map(name => name.trim().toLowerCase());

			this.allFilteredMovies = _.chain(this.allMovies)
				.filter(movie => {
					return (
						(!this.searchCriteria.title || _.includes(_.toLower(movie.title), _.toLower(this.searchCriteria.title))) &&
						(!this.searchCriteria.yearFrom || movie.year >= this.searchCriteria.yearFrom) &&
						(!this.searchCriteria.yearTo || movie.year <= this.searchCriteria.yearTo) &&
						(!this.searchCriteria.cast || _.some(movie.cast, castMember => castQueries.some(query =>
							_.includes(_.toLower(castMember), query))))
					);
				}).value();
		},
		showMore() {
			this.currentPage++;
		},
		showLess() {
			this.currentPage--;
		},
		formatList(list) {
			return list.join(', ');
		}
	},
};
</script>

<style scoped>
/* Add your CSS styling here */
</style>
