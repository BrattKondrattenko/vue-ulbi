<template>
  <div class="page__wrapper">
    <div
        v-for="pageNumber in totalPage"
        :key="pageNumber"
        class="page"
        :class="{
          'current-page': currPage === pageNumber
        }"
        @click="changeNumber(pageNumber)"
    >
      {{ pageNumber }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'my-pages',
  props: {
    totalPage: {
      type: Number
    },
    currPage: {
      type: Number
    }
  },
  methods: {
    changeNumber(pageNumber) {
        this.$emit('change', pageNumber)
    }
  }
}
</script>

<style scoped>
.page__wrapper {
  display: flex;
  margin-top: 15px;
  margin-bottom: 20px;
  margin-left: 30px;
}

.page {
  border: 1px solid teal;
  padding: 20px;
}

.current-page {
  border: 2px solid green;
}
</style>