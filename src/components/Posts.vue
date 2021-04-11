<template>
  <b-container>
    <div v-for="post in posts" :key="post.id" class="post">
      <h2>{{ post.title }}</h2>
      <i class="text-muted">by @{{ getUsernameById(post.userId) }}</i>
      <p>{{ post.body }}</p>
    </div>
  </b-container>
</template>

<script>
export default {
  name: 'Posts',
  props: {
    posts: Array,
    users: Array,
  },
  methods: {
    getUsernameById(id) {
      const user = this.users.find((u) => u.id === id);
      return user && user.username;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.post {
  text-align: left;
  margin-bottom: 1.5rem;
}
</style>
