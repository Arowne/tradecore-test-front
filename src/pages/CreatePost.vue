<template>
  <div>
    <div
      class="section section-contact-us text-center"
      data-background-color="black"
      style="height: 1000px"
    >
      <div class="container" style="margin-top: 200px">
        <h2 class="title">Want to publish a post?</h2>
        <p class="description">Your project is very important to us.</p>
        <div class="row">
          <div class="col-lg-6 text-center ml-auto mr-auto col-md-8">

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.title[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.title[0]}}</div>
            </div>
            <fg-input
              class="input-lg"
              placeholder="Title ..."
              v-model="title"
              addon-left-icon="now-ui-icons ui-1_email-85"
            >
            </fg-input>

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.image[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.image[0]}}</div>
            </div>
            <input
              type="file"
              class="input-lg"
              @change="uploadFile"
              addon-left-icon="now-ui-icons ui-1_email-85"
            />

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.content[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.content[0]}}</div>
            </div>
            <div class="textarea-container">
              <textarea
                class="form-control"
                name="name"
                rows="4"
                cols="80"
                v-model="content"
                placeholder="Type a message..."
              ></textarea>
            </div>
            <div class="send-button">
              <n-button type="primary" @click="createPost" round block size="lg"
                >Create post</n-button
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
import { Button, FormGroupInput } from "@/components";

export default {
  name: "landing",
  bodyClass: "landing-page",
  components: {
    [Button.name]: Button,
    [FormGroupInput.name]: FormGroupInput,
  },
  data() {
    return {
      title: "",
      image: null,
      content: "",
      errors: {
        title: [""],
        image: [""],
        content: [""]
      }
    };
  },
  methods: {
    uploadFile (event) {
      this.image = event.target.files[0];
    },
    clearErrorMessage() {
      for (const key in this.errors) {
        this.errors[key] = [""];
      }
    },
    createPost() {
      this.clearErrorMessage();
      const post = new FormData();
      const key = ["title", "image", "content"];
      const value = [this.title, this.image, this.content];
      const token = localStorage.getItem('aic-session-token')

      for (let i = 0; i < value.length; i++) {
        post.append(key[i], value[i]);
      }

      console.log(this.image);

      // Post value
      axios
        .post("http://localhost:9000/posts/", post, {
          headers: {
            "Authorization": "Bearer " + token
          }
        })
        .then((response) => {
          this.$router.push('/home')
        })
        .catch((error) => {
          // Clear old message
          for (const key in error.response.data.errors) {
            if (key == "detail") {
              this.errors["detail"] = [error.response.data.errors[key]];
            }
            this.errors[key] = error.response.data.errors[key];
          }
        });
    },
  }
};
</script>
<style></style>
