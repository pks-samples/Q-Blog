<template>
  <div>
    <table class="table table-information" width="100%">
      <tr>
        <td><strong>Title</strong></td>
        <td>{{ category.title }}</td>
      </tr>
      <tr>
        <td><strong>Description</strong></td>
        <td>{{ category.description }}</td>
      </tr>
      <tr>
        <td><strong>Moderator</strong></td>
        <td>{{ moderator }}</td>
      </tr>
      <tr>
        <td><strong>Total Posts</strong></td>
        <td>{{ category.total_posts }}</td>
      </tr>
    </table>

    <h4 class="mt-5">{{ category.title }},s Posts:</h4>
    <hr />
    <post-table
      :posts="posts"
      :totalPages="totalPages"
      :page="page"
      @setPage="setPage"
    ></post-table>
  </div>
</template>

<script>
import helpers from '../../../helpers.js';
import PostTable from '../posts/post_table';

export default {
  components: {
    PostTable
  },


  data: function () {
    return {
      page: 0
    }
  },


  computed: {
    category: function () {
      return this.$store.getters['categories/category'];
    },
    moderator: function () {
      if (typeof this.category.moderator === 'undefined') {
        return 'unknown';
      }
      return this.category.moderator.username;
    },
    posts: function () {
      return this.$store.getters['posts/posts'];
    },
    totalPages: function () {
      return this.$store.getters['posts/totalPages'];
    }
  },


  watch: {
    page: function (val) {
      this.$store.dispatch('posts/getCategoryPosts', {
        page: val,
        category: this.$route.params.category
      });
    }
  },


  created: function () {
    this.page = helpers.getPageQuery();
    this.$store.dispatch('categories/getCategory', this.$route.params.category);
  },


  methods: {
    setPage(page) {
      this.page = page;
      router.push('/categories/' + this.$route.params.category + '/posts?page=' + page);
    }
  }
}
</script>
