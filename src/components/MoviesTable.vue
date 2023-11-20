<template>
    <div>
      <table class="table table-striped table-bordered mb-0">
        <thead class="mdb-color white-text">
          <tr class="text-center">
            <th>Title</th>
            <th>Production Year</th>
            <th>Cast</th>
            <th>Genres</th>
          </tr>
        </thead>
        <tbody>
          <tr class="text-center" v-for="movie in movies" :key="movie.title">
            <td>{{ movie.title }}</td>
            <td>{{ movie.year }}</td>
            <td>{{ movie.cast.toString().split(",").join("\n") }}</td>
            <td>{{ movie.genres.toString().split(",").join("\n") }}</td>
          </tr>
        </tbody>
      </table>
      <button @click="showMore" class="btn btn-block mdb-color white-text mt-3">Show More</button>
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
        }
    }
};
</script>

<style scoped>
/* Add your CSS styling here */
</style>
  