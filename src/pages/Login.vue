<template>
  <div class="page-header clear-filter" filter-color="orange">
    <div
      class="page-header-image"
      style="background-image: url('img/login.jpg')"
    ></div>
    <div class="content">
      <div class="container">
        <div class="col-md-5 ml-auto mr-auto">
          <card type="login" plain>
            <div slot="header" class="logo-container">
              <img v-lazy="'img/now-logo.png'" alt="" />
            </div>

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.detail[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.detail[0]}}</div>
            </div>

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.username[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.username[0]}}</div>
            </div>

            <fg-input
              class="no-border input-lg"
              addon-left-icon="now-ui-icons users_circle-08"
              placeholder="Email..."
              v-model="username"
            >
            </fg-input>
            
            <div class="w-100 d-none pl-0 mt-4" :class="[errors.password[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.password[0]}}</div>
            </div>

            <fg-input
              type="password"
              class="no-border input-lg"
              addon-left-icon="now-ui-icons users_circle-08"
              placeholder="Password..."
              v-model="password"
            >
            </fg-input>

            <template slot="raw-content">
              <div class="card-footer text-center m-0">
                <a
                  @click="login"
                  class="btn btn-primary btn-round btn-lg btn-block"
                  >Login</a
                >
              </div>
              <div class="pull-left">
                <h6>
                  <router-link to="/register" class="link footer-link">Create Account</router-link>
                </h6>
              </div>
            </template>
          </card>
        </div>
      </div>
    </div>
    <main-footer></main-footer>
  </div>
</template>

<script>
import axios from "axios";
import { Card, Button, FormGroupInput } from "@/components";
import MainFooter from "@/layout/MainFooter";
export default {
  name: "login-page",
  bodyClass: "login-page",
  components: {
    Card,
    MainFooter,
    [Button.name]: Button,
    [FormGroupInput.name]: FormGroupInput,
  },
  data() {
    return {
      username: "",
      password: "",
      errors: {
        username: [''],
        password: [''],
        detail: [''],
      },
    };
  },
  mounted() {},
  methods: {
    clearErrorMessage() {
      for (const key in this.errors) {
        this.errors[key] = [""];
      }
    },
    login() {
      this.clearErrorMessage();
      const user = new FormData();
      const key = ["username", "password"];
      const value = [this.username, this.password];
      console.log(user)
      for (let i = 0; i < value.length; i++) {
        user.append(key[i], value[i]);
      }
      // Post value
      axios
        .post("http://localhost:9000/token", user)
        .then((res) => {
          localStorage.setItem("aic-session-token", res.data.access);
          localStorage.setItem("aic-refresh-token", res.data.refresh);
          console.log(localStorage.getItem("aic-session-token"))
          this.$router.push("/home");
        })
        .catch((error) => {
          // Clear old message
          for (const key in error.response.data) {
            if (key == "detail") {
              this.errors["detail"] = [error.response.data[key]];
            }
            this.errors[key] = error.response.data[key];
          }
          console.log(this.errors.username);
        });
    },
  },
};
</script>
