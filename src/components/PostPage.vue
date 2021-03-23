<template>
  <div class="container">
    <PostTable />
   
  </div>
</template>

<script>
import PostTable from './PostTable.vue';
export default {
  name: 'UserPage',
  props: {
    msg: String,
  },
  components: {
    PostTable,
  },
  methods: {
   
    hideModal() {
      this.$refs['create-modal'].hide();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.page-title{
  display: block;
}
</style>
