<template>
	<div class="movie-list">
		<ul class="list-group">
			<li v-for="movie in filteredMovies" :key="movie.title" class="list-group-item">
				{{ movie.title }}
			</li>
		</ul>
	</div>
</template>

<script>
import _ from 'lodash';

export default {
	name: 'MoviesByGenre',
	props: ['genre', 'movies'],
	computed: {
		filteredMovies() {
			return _.filter(this.movies, (movie) =>
				movie.genres.includes(this.genre)
			);
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
