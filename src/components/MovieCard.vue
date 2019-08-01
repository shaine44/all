<template>
  <b-card
    :title="title"
    :img-src="posterUrl"
    :img-alt="title"
    img-top
  >
    <b-card-text>{{ descr }}</b-card-text>
    <a href="#" class="card-link">Add to favorites</a>
  </b-card>
</template>

<script>
export default {
  name: 'MovieCard',
  props: {
    id: Number,
    title: String,
    posterUrl: String,
    descr: String,
    year: Number,
    countries: Array,
    genres: Array
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
