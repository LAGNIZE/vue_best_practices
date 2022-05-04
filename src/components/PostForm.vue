<template>
  <div>
    <form @submit.prevent>
      <h4>Создание поста</h4>
      <my-input
        v-model="post.title"
        class="input"
        type="text"
        placeholder="Название"
      />
      <my-input
        v-model="post.body"
        class="input"
        type="text"
        placeholder="Описание"
      />
      <my-button
        class="btn"
        v-on:click="createPost"
        style="align-self: flex-end; margin-top: 15px"
        >Создать
      </my-button>
    </form>
  </div>
</template>

<script>

export default {
  data() {
    return {
      post: {
        title: "",
        body: "",
      },
    };
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit("create", this.post);
      this.post = {
        title: "",
        body: "",
      };
    },
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
</style>
