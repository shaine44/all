<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col>
          <h1>Каталог Фильмов</h1>
        </b-col>
      </b-row>
      <b-row class="v-padded">
        <b-col>
          <label>Genre</label>
          <b-form-select id="genre-dropdown" :options="genres" v-model="selectedGenre" />
        </b-col>
        <b-col>
          <label>Country</label>
          <b-form-select id="country-dropdown" :options="countries" v-model="selectedCountry" />
        </b-col>
        <b-col>
          <label>Year</label>
          <b-form-select id="year-dropdown" :options="years" v-model="selectedYear" />
        </b-col>
      </b-row>

      <h2 v-if="filteredMovies.length === 0" class="text-center">Nothing found</h2>
      <b-card-group columns>
        <MovieCard
          v-for="movie in filteredMovies"
          :key="movie.id"
          :id="movie.id"
          :title="movie.title"
          :year="movie.year"
          :posterUrl="movie.posterUrl"
          :descr="movie.descr"
          :liked="liked.includes(movie.id)"
          :onLike="like"
        />
      </b-card-group>
    </b-container>
  </div>
</template>

<script>
import MovieCard from "./components/MovieCard.vue";
import movieDb from "./assets/data.json";
import { orderBy, concat, uniq, flattenDeep } from "lodash";
export default {
  name: "app",
  components: {
    MovieCard
  },
  mounted() {
    let likedIds = localStorage.liked;
    this.liked = likedIds ? JSON.parse(likedIds) : [];
  },
  data() {
    let movies = orderBy(movieDb, ["title"], ["asc"]);
    // чтобы получить жанры нам нужно пройтись по фильмам,
    // выцепить из каждого массив жанров, склеить все эти жанры в один массив без дубликатов
    let genres = concat(
      "any",
      orderBy(uniq(flattenDeep(movieDb.map(movie => movie.genres))))
    );
    let countries = concat(
      "any",
      orderBy(uniq(flattenDeep(movieDb.map(movie => movie.countries))))
    );
    let years = concat("any", orderBy(uniq(movieDb.map(movie => movie.year))));
    return {
      movies: movies,
      genres: genres,
      years: years,
      countries: countries,
      selectedGenre: genres[0],
      selectedCountry: countries[0],
      selectedYear: years[0],
      liked: []
    };
  },
  computed: {
    filteredMovies: function() {
      return this.movies
        .filter(
          movie =>
            movie.genres.includes(this.selectedGenre) ||
            this.selectedGenre === "any"
        )
        .filter(
          movie =>
            movie.countries.includes(this.selectedCountry) ||
            this.selectedCountry === "any"
        )
        .filter(
          movie =>
            movie.year === this.selectedYear || this.selectedYear === "any"
        );
    }
  },
  methods: {
    like(id) {
      // if we have something in localStore already
      // parse array, add new id, save it
      if (!this.liked.includes(id)) {
        this.liked.push(id);
      } else {
        this.liked = this.liked.filter(x => x !== id);
      }
      localStorage.setItem("liked", JSON.stringify(this.liked));
    }
  }
};
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 3rem;
}
.v-padded {
  padding-top: 1rem;
  padding-bottom: 1rem;
}
</style>