<template>
  <div>
    <h1>My Articles</h1>
    <router-link v-bind:to="{name: 'edit'}">New Article</router-link>

    <ul>
      <li v-for="(article, index) in articles" v-bind:key="index">
        <div>
          <router-link
            v-bind:to="{ name: 'details', params: { articleId: article._id} }"
          >{{article.title}}</router-link>
        </div>
        <div>
          <router-link v-bind:to="{ name: 'edit', params: { articleId: article._id} }">Edit Article</router-link>
          <a href v-on:click.prevent="deleteArticle(article._id)">Delete Article</a>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "MyArticles",
  data: function () {
    return {
      articles: [],
    };
  },

  components: {},

  methods: {
    deleteArticle: function (articleId) {
      this.$http
        .delete(`${process.env.VUE_APP_API_URL}articles/${articleId}`)
        .then(function () {
          this.getArticles();
        });
    },

    getArticles: function () {
      this.$http
        .get(`${process.env.VUE_APP_API_URL}users/${localStorage.userId}/articles`)
        .then(function (data) {
          console.log(data.body);
          this.articles = data.body;
        });
    },
  },

  created: function () {
    this.getArticles();
  },
};
</script>

<style scoped>
</style>
