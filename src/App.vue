<template>
  <div id="app">
    <div class="row">
      <div class="col-sm-12">
        <UserPage msg="Welcome to Your Vue.js App" />
      </div>
    </div>
  </div>
</template>

<script>
import UserPage from './components/PostPage.vue';

export default {
  name: 'App',
  components: {
    UserPage,
  },
};
</script>

<style>

</style>
