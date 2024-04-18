<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input
        v-model="searchQuery"
    />
    <div class="app_btns">
      <my-button
          style="margin-top: 15px"
          @click="showModal"
      >
        Создать пользователя
      </my-button>
      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      >

      </my-select>
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>
    <post-list
        v-if="!isPostLoading"
        :posts="sortedAndSearchQuery"
        @remove="removePost"
    />
    <div v-else>Идет загрузка...</div>
    <div ref="observer" class="observer"></div>
    <!--    <my-pages-->
    <!--      :totalPage="totalPage"-->
    <!--      :currPage="page"-->
    <!--      @change="changeNumber"-->
    <!--    />-->
  </div>
</template>

<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from "axios";
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";
import MyPages from "@/components/PostPages.vue";
export default {
  components: {MyPages, MyInput, MySelect, MyButton, MyDialog, PostList, PostForm},
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPage: 0,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По описанию'}
      ]
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter((item) => item.id !== post.id)
    },
    showModal() {
      this.dialogVisible = true
    },
    // changeNumber(pageNumber) {
    //   this.page = pageNumber
    // },

    async fetchPosts() {
      try {
        this.isPostLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data
        this.isPostLoading = false;
      } catch (e) {
        alert('Ошибка')
      }
    },
    async loadMorePosts() {
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data]
      } catch (e) {
        alert('Ошибка')
      }
    },
  },

  mounted() {
    this.fetchPosts()
    let options = {
      rootMargin: "0px",
      threshold: 1.0,
    };

    const callback = (entries, observer) => {
      if(entries[0].isIntersecting && this.page < this.totalPage) {
        this.loadMorePosts()
      }
    }

    let observer = new IntersectionObserver(callback, options);

    observer.observe(this.$refs.observer)
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1,post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchQuery() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch: {
    // page() {
    //   this.fetchPosts()
    // }
  }
}
</script>

<style>



</style>