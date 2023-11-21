<template>
	<div>
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
		<button @click="showMore" class="btn btn-info col-sm-12 mb-4 text-center">Show More</button>
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
			itemsPerPage: 10
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
	},
	computed: {
		filteredMovies() {
			return _.chain(this.allMovies)
			.filter(movie => {
					return (
						(!this.searchCriteria.title || _.includes(_.toLower(movie.title), _.toLower(this.searchCriteria.title))) &&
						(!this.searchCriteria.yearFrom || movie.year >= this.searchCriteria.yearFrom) &&
						(!this.searchCriteria.yearTo || movie.year <= this.searchCriteria.yearTo) &&
						(!this.searchCriteria.cast || _.some(movie.cast, castMember => _.includes(_.toLower(castMember), _.toLower(this.searchCriteria.cast))))
					);
				})
				.slice(0, this.itemsPerPage)
				.value();
		},
	},
	methods: {
		showMore() {
			let nextMovies = this.allMovies.slice(
				this.displayedMovies.length,
				this.displayedMovies.length + this.itemsPerPage
			);
			this.displayedMovies.push(...nextMovies);
		},
		formatList(list) {
			return list.join(', ');
		},
	},
};
</script>

<style scoped>
/* Add your CSS styling here */
</style>
