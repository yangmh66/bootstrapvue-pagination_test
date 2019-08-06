<template>
  <div id="app" class="align-items-center">
    <h2>{{title}}</h2>
    <div class="row">
      <div class="col-md-12">
        <li v-for="item in posts" :key="item.id">{{ item.id }} : {{ item.title }}</li>
      </div>
    </div>
    <br />
    <b-pagination
      size="md"
      :total-rows="100"
      v-model="currentPage"
      :per-page="10"
      @input="getPostData(currentPage)"
      align="center"
    ></b-pagination>
    <br />
    <div>currentPage: {{currentPage}}</div>
  </div>
</template>

<script>
//Import axios for REST API calls
import axios from "axios";
//Import bootstrap CSS
import "bootstrap/dist/css/bootstrap.css";
//Import bootstrap vue CSS
import "bootstrap-vue/dist/bootstrap-vue.css";

export default {
  name: "HelloWorld",
  data() {
    return {
      title: "Vue.js Pagination Example With Bootstrap",
      currentPage: 1,
      limit: 10,
      posts: []
    };
  },
  methods: {
    // Fetches posts when the component is created.
    getPostData(currentPage) {
      axios
        .get(
          "http://jsonplaceholder.typicode.com/posts?_page=" +
            this.currentPage +
            "&_limit=" +
            this.limit
        )
        .then(response => {
          console.log(response);
          // JSON responses are automatically parsed.
          this.posts = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    }
  },
  mounted(currentPage) {
    this.getPostData(currentPage);
  }
};
</script>