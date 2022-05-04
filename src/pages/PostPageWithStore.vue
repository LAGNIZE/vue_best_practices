<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input
      v-bind:model-value="searchQuery"
      v-on:update:model-value="setSearchQuery"
      placeholder="Поиск..."
    />
    <div class="app__btns">
      <my-button v-on:click="showDialog">Создать пост</my-button>
      <my-select
        v-bind:model-value="selectedSort"
        v-on:update:model-value="setSelectedSort"
        v-bind:options="sortOptions"
      ></my-select>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form v-on:create="createPost" />
    </my-dialog>
    <post-list
      v-if="!isPostLoading"
      v-on:remove="removePost"
      v-bind:posts="sortedAndSearchedPosts"
    />
    <div v-else>Идёт загрузка...</div>
    <div class="page__wrapper">
      <div
        v-for="pageNumber in totalPages"
        v-bind:key="pageNumber"
        class="page"
        v-bind:class="{ 'current-page': page === pageNumber }"
        v-on:click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyButton from "@/components/UI/MyButton.vue";
import MySelect from "@/components/UI/MySelect.vue";
import { mapState, mapGetters, mapActions, mapMutations } from "vuex";

export default {
  components: {
    PostForm,
    PostList,
    MyButton,
    MySelect,
  },
  data() {
    return {
      dialogVisible: false,
    };
  },
  methods: {
    ...mapMutations({
      setPage: "post/setPage",
      setSearchQuery: "post/setSearchQuery",
      setSelectedSort: "post/setSelectedSort"
    }),
    ...mapActions({ fetchPosts: "post/fetchPosts" }),
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    changePage(pageNumber) {
      this.page = pageNumber;
    },
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    ...mapState({
      posts: (state) => state.post.posts,
      isPostLoading: (state) => state.post.isPostLoading,
      selectedSort: (state) => state.post.selectedSort,
      searchQuery: (state) => state.post.searchQuery,
      page: (state) => state.post.page,
      limit: (state) => state.post.limit,
      totalPages: (state) => state.post.totalPages,
      sortOptions: (state) => state.post.sortOptions,
    }),
    ...mapGetters({
      sortedPost: "post/sortedPosts",
      sortedAndSearchedPosts: "post/sortedAndSearchedPosts",
    }),
  },
  watch: {},
};
</script>

<style>
.app__btns {
  display: flex;
  justify-content: space-between;
  margin: 15px 0;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
}

.current-page {
  border: 2px solid teal;
}
</style>
