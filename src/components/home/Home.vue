<template>
  <div>
    <h1>Articles</h1>
    <!-- This router link swwaps out this component for the edit article component, and doesn't pass it anything -->
    <ul>
      <!-- This <li> is being generated iteratively by looping through the 'articles' array via the v-for directive -->
      <li v-for="(article, index) in articles" v-bind:key="index">
        <div>
          <!-- 
          This router link is generated from the title of each article being iterated over
          The router link has a bound 'to' attribute, vue will programmatically interpret its contents rather than it just being treated as a string 
          It is being passed the name of a route defined oin main.js, and a route parameter named articleId that is being assigned the value of article.id, which is stored in the data object of this component
          When clicked, it intructs vue-router to switch out this component for the one specified in the path on main.js, passing it the id as a param as well-->
          <router-link
            v-bind:to="{ name: 'details', params: { articleId: article._id} }"
          >{{article.title}}</router-link>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Home",
  data: function () {
    return {
      // This is where the list of articles is stored
      articles: [],
    };
  },

  components: {},

  methods: {


    // This function sends a request to the api to retreive all the articles, and then stores them in the article property of the data object
    getArticles: function () {
      this.$http
        .get(`${process.env.VUE_APP_API_URL}articles`)
        .then(function (data) {
          console.log(data.body);
          this.articles = data.body;
        });
    },
  },

  // This lifecycle hook executes whenever this component is created, and calls the function that retreives the articles from the api
  created: function () {
    this.getArticles();
  },
};
</script>

<style scoped>
</style>
