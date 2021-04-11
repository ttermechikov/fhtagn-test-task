<template>
  <b-container class="control-bar">
    <b-row class="control-bar-row">
      <b-col
        ><strong>{{ postsInfo }}</strong>
      </b-col>
      <b-col>
        <b-row class="control-bar-row">
          <b-col>Количество:</b-col>
          <b-col
            ><b-form-select
              v-model="selected"
              v-on:change="updatePostsCount"
              :options="options"
              size="sm"
            ></b-form-select
          ></b-col>
        </b-row>
      </b-col>
      <b-col>
        <b-button variant="outline-primary">Экспорт</b-button>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: 'ControlBar',
  data() {
    return {
      selected: this.postsPerPage,
      options: [
        { value: 10, text: '10' },
        { value: 25, text: '25' },
        { value: 50, text: '50' },
      ],
    };
  },
  props: {
    postsPerPage: Number,
    postsInfo: String,
    setPostsPerPage: Function,
  },
  methods: {
    updatePostsCount() {
      this.$emit('setPostsPerPage', this.selected);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.control-bar {
  padding-bottom: 1rem;
  margin-bottom: 2rem;
  border-bottom: 1px solid #aaa;
}

.control-bar-row {
  align-items: center;
}
</style>
