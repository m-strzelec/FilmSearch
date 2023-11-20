<template>
	<div class="movie-list">
		<ul class="list-group">
			<li
				v-for="movie in filteredMovies"
				:key="movie.title"
				class="list-group-item"
			>
				{{ movie.title }}
			</li>
		</ul>
	</div>
</template>

<script>
import _ from 'lodash';

export default {
	name: 'MoviesByCast',
	props: ['cast', 'movies'],
	computed: {
		filteredMovies() {
			return _.filter(this.movies, (movie) => movie.cast.includes(this.cast));
		},
	},
};
</script>

<style scoped>
.movie-list {
	margin-top: 20px;
}

/* Further customization if needed */
</style>
