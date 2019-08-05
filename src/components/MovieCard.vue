<template>
  <b-card :title="title" :img-src="posterUrl" :img-alt="title" img-top>
    <b-card-text>{{ descr }}</b-card-text>
    <b-button :variant="likeButtonClass" @click="onLike(id)">Like</b-button>
  </b-card>
</template>

<script>
export default {
  name: "MovieCard",
  props: {
    id: Number,
    title: String,
    posterUrl: String,
    descr: String,
    year: Number,
    countries: Array,
    genres: Array,
    liked: Boolean,
    onLike: Function
  },
  computed: {
    likeButtonClass() {
      return this.liked ? "primary" : "outline-primary";
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
