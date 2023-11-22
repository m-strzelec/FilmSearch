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
			this.allFilteredMovies = _.chain(this.allMovies)
				.filter(movie => {
					// Title filter
					const titleMatch = !this.searchCriteria.title ||
						_.includes(_.toLower(movie.title), _.toLower(this.searchCriteria.title));
					// Year filters
					const yearFromMatch = !this.searchCriteria.yearFrom || movie.year >= this.searchCriteria.yearFrom;
					const yearToMatch = !this.searchCriteria.yearTo || movie.year <= this.searchCriteria.yearTo;
					// Cast filter
					let castMatch = true;
					if (this.searchCriteria.cast) {
						const castQueries = _.map(_.split(this.searchCriteria.cast, ','), name => _.toLower(_.trim(name)));
						if (this.searchCriteria.castSearchType === 'all') {
							castMatch = _.every(castQueries, query =>
								_.some(movie.cast, castMember => _.includes(_.toLower(castMember), query))
							);
						} else {
							castMatch = _.some(movie.cast, castMember =>
								_.some(castQueries, query => _.includes(_.toLower(castMember), query))
							);
						}
					}
					return titleMatch && yearFromMatch && yearToMatch && castMatch;
				})
				.value();
		},
		showMore() {
			this.currentPage++;
		},
		showLess() {
			this.currentPage--;
			//this.currentPage = 1;
		},
		formatList(list) {
			return list.join(', ');
		}
	},
};
</script>

<style scoped>
</style>
