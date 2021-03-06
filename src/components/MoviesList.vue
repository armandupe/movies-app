<template>
  <BContainer>
    <h3 class="list-title">{{ listTitle }}</h3>
    <BRow>
      <template v-if="isExist">
        <BCol sm="4" lg="6" xl="3" cols="12" v-for="(movie, key) in list" :key="key">
          <MovieItem
            :movie="movie"
            @mouseover.native="onMouseOver(movie.Poster)"
            @removeItem="onRemoveItem"
            @showModal="onShowMovieInfo"
          />
        </BCol>
      </template>
      <template v-else>
        <div>Empty list</div>
      </template>
    </BRow>
    <BModal class="movie-modal" :id="movieInfoModalID" size="xl" hide-footer hide-header>
      <MovieInfoModalContent :movie="selectedMovie" @closeModal="onCloseModal" />
    </BModal>
  </BContainer>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import MovieItem from "./MovieItem";
import MovieInfoModalContent from "./MovieInfoModalContent";

export default {
  name: "MoviesList",
  props: {
    list: {
      type: Object,
      default: () => ({}),
    },
  },
  data: () => ({
    movieInfoModalID: "movie-info",
    selectedMovieID: "",
  }),
  components: {
    MovieItem,
    MovieInfoModalContent,
  },
  methods: {
    ...mapActions("movies", ["removeMovie"]),
    ...mapActions(["showNotify"]),
    onMouseOver(poster) {
      this.$emit("changePoster", poster);
    },
    async onRemoveItem({ id, title }) {
      const isConfirmed = await this.$bvModal.msgBoxConfirm(`Are you sure delete ${title}?`);
      if (isConfirmed) {
        this.removeMovie(id);
        this.showNotify({
          msg: "Movie deleted!",
          variant: "success",
          title: "Success",
        });
      }
    },
    onShowMovieInfo(id) {
      this.selectedMovieID = id;
      this.$bvModal.show(this.movieInfoModalID);
    },
    onCloseModal() {
      this.selectedMovieID = null;
      this.$bvModal.hide(this.movieInfoModalID);
    },
  },
  computed: {
    ...mapGetters("movies", ["isSearch"]),
    isExist() {
      return Boolean(Object.keys(this.list).length);
    },
    listTitle() {
      return this.isSearch ? "Search result" : "IMDB Top 250";
    },
    selectedMovie() {
      return this.selectedMovieID ? this.list[this.selectedMovieID] : null;
    },
  },
};
</script>

<style lang="scss" scoped>
.list-title {
  color: azure;
  font-size: 5rem;
  margin-bottom: 3rem;
}
</style>

<style lang="scss">
.movie-modal-body {
  padding: 0 !important;
}
</style>
