<template>
  <div>
    <div class="page-header clear-filter" filter-color="orange">
      <parallax
        class="page-header-image"
        :style="'background-image:url(http://0.0.0.0:9000' + post.image + ')'"
      >
      </parallax>
      <div class="container">
        <h3 class="title">
          {{ post.user.first_name }} {{ post.user.last_name }}
        </h3>
      </div>
    </div>
    <div class="section">
      <div class="container">
        <div class="button-container">
          <a
            class="btn btn-default btn-round btn-lg btn-icon pt-1"
            style="font-size: 20px"
            rel="tooltip"
            @click="likePost"
            :class="[post.user_like ? 'btn-primary' : '']"
          >
            <p style="font-size: 12px; margin-bottom: 0px">{{ post.like }}</p>
            <i class="fas fa-thumbs-up"></i>
          </a>
          <a
            class="btn btn-default btn-round btn-lg btn-icon pt-1"
            style="font-size: 25px"
            rel="tooltip"
            @click="unlikePost"
            :class="[post.user_unlike ? 'btn-primary' : '']"
          >
            <p style="font-size: 12px; margin-bottom: 0px">{{ post.unlike }}</p>
            <i class="fas fa-thumbs-down"></i>
          </a>
        </div>
        <h3 class="title">{{ post.title }}</h3>
        <h5 class="description">
          {{ post.content }}
        </h5>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "profile",
  bodyClass: "profile-page",
  data() {
    return {
      postId: this.$route.params.id,
      post: [],
    };
  },
  mounted() {
    this.getPost();
  },
  methods: {
    getPost() {
      axios
        .get("http://localhost:9000/posts/" + this.postId, {
          headers: {
            Authorization:
              "Bearer " + localStorage.getItem("aic-session-token"),
          },
        })
        .then((response) => {
          console.log(response.data);
          this.post = response.data;
        });
    },
    likePost() {
      axios({
        method: "post", //you can set what request you want to be
        url: "http://localhost:9000/posts/like/" + this.postId,
        headers: {
          Authorization: "Bearer " + localStorage.getItem("aic-session-token"),
        },
      }).then((response) => {
        console.log(response.data);
        this.getPost();
      });
    },
    unlikePost() {
      axios({
        method: "post", //you can set what request you want to be
        url: "http://localhost:9000/posts/unlike/" + this.postId,
        headers: {
          Authorization: "Bearer " + localStorage.getItem("aic-session-token"),
        },
      }).then((response) => {
        console.log(response.data);
        this.getPost();
      });
    },
  },
};
</script>
<style></style>
