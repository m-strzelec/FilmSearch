<template>
	<div id="app">
		<app-navbar></app-navbar>
		<main>
			<search-component @search="handleSearch" @clear="handleSearch"></search-component>
			<movies-table :all-movies="allMovies" :search-criteria="searchCriteria"
				@filtered="filteredMovies"></movies-table>
			<h2>Movies By Genre</h2>
			<movies-by-genre :movies="filteredM"></movies-by-genre>
			<h2>Movies By Cast</h2>
			<movies-by-cast :movies="filteredM"></movies-by-cast>
		</main>
		<app-footer></app-footer>
	</div>
</template>

<script>
import AppNavbar from './components/AppNavbar.vue';
import AppFooter from './components/AppFooter.vue';
import SearchComponent from './components/SearchComponent.vue';
import MoviesTable from './components/MoviesTable.vue';
import MoviesByGenre from './components/MoviesByGenre.vue';
import MoviesByCast from './components/MoviesByCast.vue';
import movies from './data/movies.json';

export default {
	name: 'App',
	components: {
    AppNavbar,
    AppFooter,
    SearchComponent,
    MoviesTable,
    MoviesByGenre,
    MoviesByCast,
},
	data() {
		return {
			allMovies: movies,
			filteredM: [],
			searchCriteria: {
				title: '',
				yearFrom: null,
				yearTo: null,
				cast: '',
				castSearchType: 'any'
			},
		};
	},
	methods: {
		handleSearch(criteria) {
			this.searchCriteria = criteria;
		},
		filteredMovies(filtered) {
			this.filteredM = filtered;
		}
	},
};
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
	margin-top: 80px;
	margin-left: 50px;
	margin-right: 50px;
}

h1,
h2 {
	text-align: center;
}

h2 {
	padding-top: 20px;
}
</style>
