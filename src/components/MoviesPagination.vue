<template>
  <div class="movies-pagination d-flex justify-content-center">
    <BPagination v-model="currentPageModel" :per-page="perPage" :total-rows="total" prev-text="Prev" next-text="Next" />
  </div>
</template>

<script>
export default {
  name: "MoviesPagination",
  props: {
    currentPage: {
      type: Number,
      default: 1,
    },
    total: {
      type: Number,
      default: 1,
    },
    perPage: {
      type: Number,
      default: 1,
    },
  },
  computed: {
    currentPageModel: {
      get() {
        return this.currentPage;
      },
      set(value) {
        this.$emit("pageChanged", value);
      },
    },
  },
};
</script>

<style scoped lang="scss">
.movies-pagination {
  margin-top: auto;

  &::v-deep .pagination .page-item .page-link {
    background-color: transparent;
    font-size: 1rem;
    color: #fff;
    box-shadow: none;
  }
  &::v-deep .pagination .page-item.active .page-link {
    border-color: #fff;
    background-color: #fff;
    color: #000;
  }

  &::v-deep .pagination .page-item.disabled .page-link {
    color: lightgray;
  }
}
</style>
