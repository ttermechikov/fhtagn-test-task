<template>
  <b-container>
    <b-row>
      <b-col>
        <b-pagination
          v-model="currentPage"
          :total-rows="totalRows"
          :per-page="perPage"
          @change="updateCurrentPage"
          align="center"
          limit="1"
          prev-text="Предыдущая"
          next-text="Следующая"
        ></b-pagination>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: 'Pagination',
  props: {
    currentPage: Number,
    totalRows: Number,
    perPage: Number,
  },
  methods: {
    updateCurrentPage(page) {
      this.$emit('setCurrentPage', page);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
