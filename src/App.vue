<template>
  <div id="app">
    <b-alert v-if="error" variant="danger" show>{{ error }}</b-alert>
    <div v-else>
      <ControlBar
        :postsPerPage="postsPerPage"
        :postsInfo="postsInfo"
        :posts="postsForRender"
        @setPostsPerPage="setPostsPerPage"
      />
      <Pagination
        :currentPage="currentPage"
        :total-rows="postsCount"
        :per-page="postsPerPage"
        @setCurrentPage="setCurrentPage"
      />
      <Posts :posts="postsForRender" :users="users" />
      <Pagination
        :currentPage="currentPage"
        :total-rows="postsCount"
        :per-page="postsPerPage"
        @setCurrentPage="setCurrentPage"
      />
    </div>
  </div>
</template>

<script>
import ControlBar from './components/ControlBar.vue';
import Pagination from './components/Pagination.vue';
import Posts from './components/Posts.vue';

const API_URL = 'https://jsonplaceholder.typicode.com';

export default {
  name: 'App',
  data() {
    return {
      postsPerPage: 10,
      currentPage: 1,
      posts: [],
      users: [],
      error: '',
    };
  },
  components: {
    ControlBar,
    Pagination,
    Posts,
  },
  methods: {
    setPostsPerPage(count) {
      if (Number.isInteger(count) && count > 0) {
        this.postsPerPage = count;
        this.currentPage = 1;
      }
    },
    setCurrentPage(page) {
      if (Number.isInteger(page) && page > 0) {
        this.currentPage = page;
      }
    },
    getCurrentPostsIndexes() {
      const { postsPerPage, currentPage } = this;
      const startInd = postsPerPage * (currentPage - 1);
      const endInd = startInd + postsPerPage;

      return { startInd, endInd };
    },
  },
  computed: {
    postsCount() {
      return this.posts.length;
    },
    postsForRender() {
      const { startInd, endInd } = this.getCurrentPostsIndexes();
      return this.posts.slice(startInd, endInd);
    },
    postsInfo() {
      const { startInd, endInd } = this.getCurrentPostsIndexes();
      return `Всего: ${this.posts.length}, показаны: ${startInd + // eslint-disable-line
        1} - ${endInd}`;
    },
  },
  mounted() {
    const getPostsPromise = fetch(`${API_URL}/posts`);
    const getUsersPromise = fetch(`${API_URL}/users`);

    Promise.all([getPostsPromise, getUsersPromise])
      .then(
        ([postsRes, usersRes]) =>
          Promise.all([postsRes.json(), usersRes.json()]) // eslint-disable-line
      )
      .then(([posts, users]) => {
        this.posts = posts;
        this.users = users;
      })
      .catch((e) => {
        console.log('Error while fetch data', e);
        this.error = e;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 2rem;
  min-height: 100vh;
}
</style>
