<template>
  <div class="section section-examples" data-background-color="black" style="min-height: 1000px">
    <div class="space-50"></div>
    <div class="container text-center">
      <div class="row">
        <div class="col-md-6" v-for="post in posts" :key="post.public_id">
          <router-link :to="'/detail/' + post.public_id">
            <img :src="'http://0.0.0.0:9000' + post.image" alt="Image" class="img-raised" />
          </router-link>
          <br/>
          <router-link
            :to="'/detail/' + post.public_id"
            class="btn btn-simple btn-primary btn-round"
            >
            {{post.title}}
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      slide: "first",
      trending_slide: 1,
      latest_slide: 1,
      posts: []
    };
  },
  beforeMount() {
    this.listPosts();
  },
  mounted() {
    console.log(this.favoriteList);
  },
  methods: {
    listPosts () {
      let token = localStorage.getItem("aic-session-token");
      
      axios({
        method: "get", //you can set what request you want to be
        url: "http://localhost:9000/posts/all"
      }).then((response) => {
          this.posts = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
  },
};
</script>
<style></style>
