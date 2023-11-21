<template>
	<div id="app">
		<h1>Movies Database</h1>
		<search-component @search="handleSearch" @clear="handleSearch"></search-component>
		<movies-table :all-movies="allMovies" :search-criteria="searchCriteria"
			@filtered="updateFilteredMovies"></movies-table>

		<h2>Movies by genre</h2>
		<select v-model="selectedGenre">
			<option v-for="genre in genres" :value="genre" :key="genre">
				{{ genre }}
			</option>
		</select>
		<movies-by-genre :genre="selectedGenre" :movies="moviesByGenre"></movies-by-genre>

		<h2>Movies by cast</h2>
		<select v-model="selectedCast">
			<option v-for="cast in casts" :value="cast" :key="cast">
				{{ cast }}
			</option>
		</select>
		<movies-by-cast :cast="selectedCast" :movies="moviesByCast"></movies-by-cast>
	</div>
</template>

<script>
import SearchComponent from './components/SearchComponent.vue';
import MoviesTable from './components/MoviesTable.vue';
import MoviesByGenre from './components/MoviesByGenre.vue';
import MoviesByCast from './components/MoviesByCast.vue';
import movies from './data/movies.json';

export default {
	name: 'App',
	components: {
		SearchComponent,
		MoviesTable,
		MoviesByGenre,
		MoviesByCast,
	},
	data() {
		return {
			allMovies: movies,
			searchCriteria: {
				title: '',
				yearFrom: null,
				yearTo: null,
				cast: ''
			},
		};
	},
	methods: {
		handleSearch(criteria) {
			this.searchCriteria = criteria;
		},
	},
};
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
	margin-top: 60px;
	margin-bottom: 60px;
	padding-left: 50px;
	padding-right: 50px;
}

h1,
h2 {
	text-align: center;
}

h2 {
	padding-top: 10px;
}
</style>
