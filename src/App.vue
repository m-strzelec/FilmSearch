<template>
    <div id="app">
        <search-component @search="handleSearch"></search-component>
        <movies-table :movies="displayedMovies" @show-more="loadMoreMovies"></movies-table>

        <select v-model="selectedGenre">
            <option v-for="genre in genres" :value="genre" :key="genre">
                {{ genre }}
            </option>
        </select>
        <movies-by-genre :genre="selectedGenre" :movies="moviesByGenre"></movies-by-genre>

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
import _ from 'lodash';
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
            displayedMovies: [],
            moviesByGenre: [],
            moviesByCast: [],
            selectedGenre: '',
            selectedCast: '',
            itemsPerPage: 10,
            searchQuery: '',
            genres: [],
            casts: [],
        };
    },
    watch: {
        searchQuery() {
            this.filterMovies();
        },
        selectedGenre(newGenre) {
            this.moviesByGenre = _.filter(this.allMovies, movie => movie.genres.includes(newGenre)).slice(0, 100);
        },
        selectedCast(newCast) {
            this.moviesByCast = _.filter(this.allMovies, movie => movie.cast.includes(newCast)).slice(0, 100);
        },
    },
    created() {
        this.initializeMovieLists();
    },
    methods: {
        handleSearch(query) {
            this.searchQuery = query;
        },
        filterMovies() {
            if (this.searchQuery) {
                this.displayedMovies = _.filter(this.allMovies, (movie) =>
                    movie.title.toLowerCase().includes(this.searchQuery.toLowerCase())
                );
            } else {
                this.displayedMovies = this.allMovies.slice(0, this.itemsPerPage);
            }
        },
        loadMoreMovies() {
            let currentLength = this.displayedMovies.length;
            let moreMovies = this.allMovies.slice(
                currentLength,
                currentLength + this.itemsPerPage
            );
            this.displayedMovies = this.displayedMovies.concat(moreMovies);
        },
        initializeMovieLists() {
            this.displayedMovies = this.allMovies.slice(0, this.itemsPerPage);

            this.genres = _.uniq(_.flatten(_.map(this.allMovies, 'genres'))).slice(
                0,
                100
            );
            this.casts = _.uniq(_.flatten(_.map(this.allMovies, 'cast'))).slice(
                0,
                100
            );

            this.moviesByGenre = _.filter(this.allMovies, (movie) =>
                this.genres.includes(movie.genres[0])
            ).slice(0, 100);
            this.moviesByCast = _.filter(this.allMovies, (movie) =>
                this.casts.includes(movie.cast[0])
            ).slice(0, 100);
        },
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
