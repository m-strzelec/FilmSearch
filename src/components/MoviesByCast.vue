<template>
	<div class="my-3">
		<div class="form-group input-center row my-3">
			<label for="inputFilterCast">Cast:</label>
			<div class="col-sm-12">
				<input type="text" v-model="filterCast" id="inputFilterCast" class="form-control"
					placeholder="First and last name" />
			</div>
		</div>
		<div v-for="(movies, castMember) in moviesByCast" :key="castMember" class="card">
			<div class="card-header border-dark">{{ castMember }}</div>
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
	name: 'MoviesByCast',
	props: {
		movies: Array
	},
	data() {
		return {
			filterCast: '',
		};
	},
	computed: {
		moviesByCast() {
			const grouped = {};
			this.movies.forEach(movie => {
				movie.cast.forEach(castMember => {
					if (!grouped[castMember]) {
						grouped[castMember] = [];
					}
					grouped[castMember].push(movie);
				});
			});

			const filteredGrouped = this.filterCast
				? _.pickBy(grouped, (_movies, castMember) =>
					_.toLower(castMember).includes(_.toLower(this.filterCast))
				)
				: grouped;

			_.forEach(filteredGrouped, (movies, castMember) => {
				filteredGrouped[castMember] = _.orderBy(movies, 'title', 'asc');
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
