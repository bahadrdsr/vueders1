<template>
  <div>
 
    <b-form @submit="sendData" @reset="reset">
      <b-form-group id="input-group-2" label="Başlık:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="post.title"
          placeholder="Başlık giriniz"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="İçerik:" label-for="input-2">
        <b-form-textarea
          id="input-2"
          v-model="post.body"
          placeholder="İçerik giriniz"
          rows="10"
          required
        ></b-form-textarea>
      </b-form-group>
      <b-button type="submit" variant="primary">Gönder</b-button>
      <b-button type="reset" variant="danger">Sıfırla</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  name: 'UserForm',
  props: {
    editPost: {
      type: Object,
      default: function() {
        return { id: '', title: '', body: '' };
      },
    },
    update: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      post: this.editPost || { id: '', title: '', body: '' },
    };
  },
  methods: {
    sendData(event) {
      event.preventDefault();
      let requestOptions = {};
      if (this.update) {
        requestOptions = {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(this.post),
        };
        fetch(
          `https://jsonplaceholder.typicode.com/posts/${this.post.id}`,
          requestOptions
        )
          .then((response) => response.json())
          .then((data) => {
            this.$emit('updated', data);
          });
      } else {
        requestOptions = {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(this.post),
        };
        fetch(`https://jsonplaceholder.typicode.com/posts`, requestOptions)
          .then((response) => response.json())
          .then((data) => {
            this.$emit('created', data);
          });
      }
    },
    reset(event) {
      event.preventDefault();
      this.post.id = '';
      this.post.title = '';
      this.post.body = '';
    },
  },
};
</script>
