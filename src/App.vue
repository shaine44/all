<template>
  <div id="app">
    <b-container>
      <b-row >
        <b-col v-for="movie in movies" :key="movie.id" sm="4">
          <MovieCard
            :title="movie.title"
            :year="movie.year"
            :posterUrl="movie.posterUrl"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import MovieCard from './components/MovieCard.vue'
import movieDb from './assets/data.json'
import { orderBy } from 'lodash'

export default {
  name: 'app',
  components: {
    MovieCard
  },
  data () {
    return {
      movies: orderBy(movieDb, ['title'], ['asc'])
    }
  },
  methods: {
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
