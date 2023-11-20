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
          <tr class="text-center" v-for="(movie, index) in movies" :key="movie.title">
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
export default {
    name: 'MoviesTable',
    props: {
        movies: Array
    },
    methods: {
        showMore() {
            this.$emit('show-more');
        },
        formatList(list) {
            return list.join(', ');
        }
    }
};
</script>

<style scoped>
/* Add your CSS styling here */
</style>
  