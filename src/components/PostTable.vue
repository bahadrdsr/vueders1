<template>
  <div class="container-fluid">
    <div class="row d-flex justify-content-between align-items-center">
      <h3 class="page-title my-3">Kullanıcılar</h3>
      <div>
        <b-button id="show-btn" @click="create">Yeni</b-button>
      </div>
    </div>
    <b-table striped hover :items="posts" :fields="fields" show-empty>
      <template #cell(actions)="row">
        <b-button size="sm" @click="edit(row.item)" class="mr-1">
          Edit
        </b-button>
        <b-button
          size="sm"
          @click="delete (row.item, row.index, $event.target)"
          class="mr-1"
        >
          Delete
        </b-button>
      </template>
    </b-table>
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
import PostForm from './PostForm.vue';
import Vue from 'vue';
export default {
  name: 'PostTable',
  components: {
    PostForm,
  },
  data() {
    return {
      posts: [],
      selectedPost: { id: '', title: '', body: '' },
      fields: [
        {
          key: 'id',
          label: 'ID',
          sortable: true,
          sortDirection: 'desc',
        },
        {
          key: 'title',
          label: 'Başlık',
          sortable: true,
          sortDirection: 'desc',
        },
        {
          key: 'body',
          label: 'İçerik',
          sortable: true,
          sortDirection: 'desc',
        },
        { key: 'actions', label: 'Actions' },
      ],
      formModal: {
        id: 'edit-modal',
        title: '',
        content: '',
      },
      deleteConfirmModal: {
        id: 'delete-modal',
        title: '',
        content: '',
      },
    };
  },
  created() {
    // GET request using fetch with set headers
    const headers = { 'Content-Type': 'application/json' };
    fetch('https://jsonplaceholder.typicode.com/posts', { headers })
      .then((response) => response.json())
      .then((data) => (this.posts = data));
  },
  methods: {
    edit(item) {
      this.selectedPost = { ...item };
      this.$refs['edit-modal'].show();
    },
    create() {
      this.$refs['create-modal'].show();
    },
    delete() {},
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
        this.$refs['edit-modal'].hide();
        this.$bvToast.toast(
          `${savedData.id} kimlikli post başarılıyla düzenlendi`,
          {
            title: `Tebrikler `,
            variant: 'success',
            solid: true,
          }
        );
      }
    },
    onCreated(savedData) {
      if (savedData.id) {
        this.posts.unshift(savedData);
        this.$refs['create-modal'].hide();
        this.$bvToast.toast(
          `${savedData.id} kimliği ile post başarılıyla oluşturuldu`,
          {
            title: `Tebrikler`,
            variant: 'success',
            solid: true,
          }
        );
      }
    },
  },
};
</script>
