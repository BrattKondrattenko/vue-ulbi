<template>
  <div class="post">
    <div>
      <div><strong>Название:</strong> {{ post.id }}</div>
      <div><strong>Название:</strong> {{ post.title }}</div>
      <div><strong>Описание:</strong> {{ post.body }}</div>
    </div>
    <div class="post__btns">
      <my-button
          class="btn"
          style="margin-top: 15px"
          @click="$emit('remove', post)"
      >
        Удалить
      </my-button>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  }
}
</script>

<style scoped>
.post {
  padding: 10px 15px;
  margin-bottom: 10px;
  border: 2px solid teal;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>