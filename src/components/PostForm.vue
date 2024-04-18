<template>
  <form class="form" @submit.prevent >
    <h3>Создание поста</h3>
    <my-input
        v-model = "post.title"
        placeholder="Название поста"
        type="text"
    />
    <my-input
        v-model="post.body"
        placeholder="Описание поста"
        type="text"
    />
    <my-button
        @click="createPosts"
        class="btn">
      Добавить пост
    </my-button>
  </form>
</template>

<script>
export default {
    data() {
      return {
        post: {
          title: '',
          body: ''
        },
      }
    },
  methods: {
      createPosts() {
        this.post.id = Date.now()
        this.$emit('create', this.post)
        this.post = {
          title: '',
          body: ''
        }
      }
  },
  watch: {
    post: {
      handler(newVal) {
        console.log(newVal)
      },
      deep: true
    }
  }
}
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  gap: 20px;
  text-align: center;
}


</style>