<template>
  <div class="movie-item mb-3">
    <div class="movie-item-poster" :style="posterBg"></div>
    <div class="movie-info-wrap d-flex flex-column justify-content-between">
      <div class="movie-item-info">
        <h3 class="movie-title">{{ movie.Title }}</h3>
        <span class="movie-year">{{ movie.Year }}</span>
      </div>
      <div class="movie-item-controls row no-gutters">
        <div class="col pr-0 pr-lg-2 pb-2">
          <BButton size="md" @click="showInfoModalEvent" block variant="outline-light">Инфо</BButton>
        </div>
        <div class="col pl-0 pl-lg-2">
          <BButton size="md" block variant="outline-light" @click="emitRemoveEvent">Удалить</BButton>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MovieItem",
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
  computed: {
    posterBg() {
      return {
        "background-image": `url(${this.movie.Poster})`,
      };
    },
  },
  methods: {
    emitRemoveEvent() {
      this.$emit("removeItem", {
        id: this.movie.imdbID,
        title: this.movie.Title,
      });
    },
    showInfoModalEvent() {
      this.$emit("showModal", this.movie.imdbID);
    },
  },
};
</script>

<style lang="scss" scoped>
.movie {
  &-item {
    position: relative;
    cursor: pointer;
    border-radius: 5px;
    overflow: hidden;
    transition: all 0.2s ease;
    height: 400px;
    &:hover {
      box-shadow: 0 5px 30px rgba(0, 0, 0, 0.7);
      transform: scale(1.02);
    }
    &:hover .movie-info-wrap {
      opacity: 1;
      background-color: rgba(0, 0, 0, 0.7);
    }
  }

  &-info-wrap {
    padding: 20px 30px;
    height: 100%;
    opacity: 0;
    transition: all 0.2s ease;
  }

  &-item-poster {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center center;
    z-index: -1;
  }

  &-title {
    font-style: 20px;
    color: #fff;
    word-break: break-word;
  }

  &-year {
    font-style: 14px;
    color: #fff;
  }
}
</style>
