<template>
  <div id="app">
    <Loader />
    <Notification />
    <PosterBg :poster="posterBg" />
    <Header />
    <MoviesList :list='moviesList' @changePoster="onChangePoster" />
    <Pagination
      :currentPage='currentPage'
      :perPage='moviesPerPage'
      :total="moviesLength"
      @pageChanged="onPageChanged"
    />
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
import MoviesList from '@/components/MoviesList.vue';
import PosterBg from '@/components/PosterBg.vue';
import Pagination from '@/components/Pagination.vue';
import Loader from '@/components/Loader.vue';
import Header from '@/components/Header.vue';
import Notification from '@/components/Notification.vue';

export default {
  name: 'App',
  components: {
    MoviesList,
    PosterBg,
    Pagination,
    Loader,
    Header,
    Notification,
  },
  data: () => ({
    posterBg: '',
  }),
  computed: {
    ...mapGetters('movies', [
      'moviesList',
      'currentPage',
      'moviesPerPage',
      'moviesLength',
    ]),
  },
  watch: {
    '$route.query': {
      handler: 'onPageQueryChange',
      immediate: true,
      deep: true,
    },
  },
  methods: {
    ...mapActions('movies', ['changeCurrentPage']),
    onPageQueryChange({ page = 1 }) {
      this.changeCurrentPage(Number(page));
    },
    onChangePoster(poster) {
      this.posterBg = poster;
    },
    onPageChanged(page) {
      this.$router.push({ query: { page } });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: relative;
}
</style>
