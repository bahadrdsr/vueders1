<template>
  <div class="container-fluid">
    <div class="row d-flex justify-content-between align-items-center">
      <h3 class="page-title my-3">Posts</h3>
      <div>
        <b-button id="show-btn" @click="create">Yeni</b-button>
      </div>
    </div>
    <div class="row">
      <b-form-select
        v-model="selectedUser"
        :options="users"
        class="mb-3"
        value-field="id"
        text-field="name"
        @change="handleUserSelect"
      >
        <template #first>
          <b-form-select-option :value="null" disabled
            >PLEASE SELECT USER BLYED</b-form-select-option
          >
        </template>
      </b-form-select>
    </div>
    <div class="row">
      <b-card
        :title="post.title"
        v-for="post in filteredPosts"
        :key="post.id"
        header-tag="header"
        tag="article"
        class="mb-4 w-100"
      >
        <template #header>
          <div class="float-right">
            <b-button-group>
              <b-button @click="edit(post)" class="ml-1">EDIT</b-button>
              <b-button @click="deletePost(post)" class="ml-1">DELETE</b-button>
            </b-button-group>
          </div>
        </template>
        <b-card-text>
          <p class="lead">{{ post.body }}</p>
        </b-card-text>
      </b-card>
    </div>
    <b-modal ref="edit-modal" hide-footer title="Post Düzenle">
      <postForm
        v-bind:editPost="selectedPost"
        update
        v-on:updated="onUpdated"
      />
    </b-modal>
    <b-modal ref="create-modal" hide-footer title="Post Oluştur">
      <PostForm v-on:created="onCreated" />
    </b-modal>
  </div>
</template>

<script>
import PostForm from "./PostForm.vue";
import Vue from "vue";
export default {
  name: "PostList",
  components: {
    PostForm,
  },
  data() {
    return {
      posts: [],
      filteredPosts: [],
      selectedPost: { id: "", title: "", body: "" },
      users: [],
      selectedUser: null,
      formModal: {
        id: "edit-modal",
        title: "",
        content: "",
      },
      deleteConfirmModal: {
        id: "delete-modal",
        title: "",
        content: "",
      },
    };
  },
  created() {
    // GET request using fetch with set headers
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((json) => {
        this.users = json;
        console.log(json);
      });
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then((response) => response.json())
      .then((json) => {
        this.posts = json;
        this.filteredPosts = json;
        console.log(json);
      });
  },
  methods: {
    edit(item) {
      this.selectedPost = { ...item };
      this.$refs["edit-modal"].show();
    },
    create() {
      this.$refs["create-modal"].show();
    },
    deletePost() {},
    handleUserSelect() {
      this.filteredPosts = this.posts.filter(
        (x) => x.userId === this.selectedUser
      );
    },
    onUpdated(savedData) {
      if (savedData.id) {
        this.posts[
          this.posts.findIndex((x) => x.id == savedData.id)
        ] = savedData;
        Vue.set(
          this.posts,
          this.posts.findIndex((x) => x.id == savedData.id),
          savedData
        );
        this.$refs["edit-modal"].hide();
        this.$bvToast.toast(
          `${savedData.id} kimlikli post başarılıyla düzenlendi`,
          {
            title: `Tebrikler `,
            variant: "success",
            solid: true,
          }
        );
      }
    },
    onCreated(savedData) {
      if (savedData.id) {
        this.selectedUser = null;
        this.filteredPosts.unshift(savedData);
        this.$refs["create-modal"].hide();
        this.$bvToast.toast(
          `${savedData.id} kimliği ile post başarılıyla oluşturuldu`,
          {
            title: `Tebrikler`,
            variant: "success",
            solid: true,
          }
        );
      }
    },
  },
};
</script>
