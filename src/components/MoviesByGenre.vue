<template>
	<div class="my-3">
		<div class="form-group input-center row my-3">
			<label for="inputFilterGenre">Genre:</label>
			<div class="col-sm-12">
				<input type="text" v-model="filterGenre" id="inputFilterGenre" class="form-control"
					placeholder="Move genre name" />
			</div>
		</div>
		<div v-for="(movies, genre) in moviesByGenre" :key="genre" class="card">
			<div class="card-header border-dark">{{ genre }}</div>
			<ol class="list-group list-group-numbered">
				<li v-for="(movie, index) in movies" :key="index" class="list-group-item list-group-item-action">
					{{ movie.title }}
				</li>
			</ol>
		</div>
	</div>
</template>

<script>
import _ from 'lodash';

export default {
	name: 'MoviesByGenre',
	props: {
		movies: Array
	},
	data() {
		return {
			filterGenre: '',
		};
	},
	computed: {
		moviesByGenre() {
			const grouped = {};
			this.movies.forEach(movie => {
				movie.genres.forEach(genre => {
					if (!grouped[genre]) {
						grouped[genre] = [];
					}
					grouped[genre].push(movie);
				});
			});

			const filteredGrouped = this.filterGenre
				? _.pickBy(grouped, (_movies, genre) =>
					_.toLower(genre).includes(_.toLower(this.filterGenre))
				)
				: grouped;

			_.forEach(filteredGrouped, (movies, genre) => {
				filteredGrouped[genre] = _.orderBy(movies, 'title', 'asc');
			});

			return _(filteredGrouped)
				.toPairs()
				.orderBy([pair => pair[1].length, pair => pair[0]], ['desc', 'asc'])
				.fromPairs()
				.value();
		}
	}
};
</script>

<style scoped></style>


  