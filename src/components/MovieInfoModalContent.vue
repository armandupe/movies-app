<template>
  <div class="movie-info-wrap">
    <header class="movie-info-header">
      <h6 class="movie-header-title">Информация о фильме</h6>
      <BIconX class="close-icon" @click="closeModal" />
    </header>
    <div class="movie-info-content">
      <BRow>
        <BCol sm="4">
          <div class="movie-poster-wrap">
            <div class="movie-poster" :style="posterStyle"></div>
          </div>
        </BCol>
        <BCol sm="8">
          <h3 class="movie-title">{{ movie.Title }}</h3>
          <BFormRating
            class="movie-rating flex-wrap my-3 my-lg-0"
            no-border
            show-value-max
            stars="10"
            precision="1"
            show-value
            readonly
            v-model="movie.imdbRating"
          />
          <p class="movie-description">{{ movie.Plot }}</p>
          <div class="mt-3 mb-4">
            <BBadge class="mr-2 mb-2" variant="success">{{ movie.Year }}</BBadge>
            <BBadge class="mr-2 mb-2" variant="success">{{ movie.Runtime }}</BBadge>
            <BBadge class="mr-2 mb-2" variant="success">{{ movie.Genre }}</BBadge>
            <BBadge class="mr-2 mb-2" variant="success">{{ movie.Language }}</BBadge>
          </div>
          <table class="table small">
            <tbody>
              <tr>
                <th>Production</th>
                <td>{{ movie.Production }}</td>
              </tr>
              <tr>
                <th>Country</th>
                <td>{{ movie.Country }}</td>
              </tr>
              <tr>
                <th>Director</th>
                <td>{{ movie.Director }}</td>
              </tr>
              <tr>
                <th>Writer</th>
                <td>{{ movie.Writer }}</td>
              </tr>
              <tr>
                <th>Actors</th>
                <td>{{ movie.Actors }}</td>
              </tr>
              <tr>
                <th>Awards</th>
                <td>{{ movie.Awards }}</td>
              </tr>
            </tbody>
          </table>
        </BCol>
      </BRow>
    </div>
  </div>
</template>

<script>
export default {
  name: "MovieInfoModalContent",
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
  data: () => ({
    defaultPosterBg: "linear-gradient(45deg, rgb(0, 3, 38) 0%, rgb(82, 15, 177) 100%)",
  }),
  computed: {
    posterStyle() {
      return {
        "background-image": this.posterBg,
      };
    },
    posterBg() {
      return this.movie ? `url(${this.movie.Poster})` : this.defaultPosterBg;
    },
  },
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
  },
};
</script>

<style lang="scss" scoped>
.movie {
  &-info-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    background: linear-gradient(45deg, rgb(0, 3, 38) 0%, rgb(82, 15, 117) 100%);
    color: #fff;
  }

  &-header-title {
    margin-bottom: 0;
    line-height: 1.5;
    font-size: 1.25rem;
  }

  &-info-content {
    padding: 1rem;
    background-color: #fff;
    &::v-deep .badge {
      padding: 0.5rem 0.5rem;
      border-radius: 0.8rem;
      font-weight: 600;
    }
  }

  &-poster-wrap {
    position: relative;
    padding-bottom: 150%;
    border-radius: 5px;
    overflow: hidden;
    transition: all 0.2s ease;
  }

  &-poster {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center center;
  }

  &-title {
    font-size: 3.5rem;
    font-weight: 300;
    line-height: 1.2;
  }

  &-rating {
    padding: 0;
  }

  &-rating:focus {
    box-shadow: none;
  }

  &-rating::v-deep .b-rating-star,
  &-rating::v-deep .b-rating-value {
    justify-content: flex-start;
    flex-grow: 0 !important;
    font-size: 1.3rem;
    font-weight: 300;
    padding: 0;
  }

  &-rating::v-deep .b-rating-star + .b-rating-star {
    margin-left: 4px;
  }

  &-rating::v-deep .b-rating-value {
    margin-left: 10px;
  }

  &-rating::v-deep .b-rating-star .b-rating-icon {
    color: #ffc107;
  }

  &-description {
    font-size: 1.25rem;
    font-weight: 300;
  }
}

.close-icon {
  font-size: 24px;
  cursor: pointer;
}
</style>
