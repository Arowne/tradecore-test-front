<template>
  <div
    class="section section-signup"
    style="background-image: url('img/bg11.jpg'); background-size: cover; background-position: top center; min-height: 1000px;"
  >
    <div class="container">
      <div class="row">
        <card class="card-signup" header-classes="text-center" color="orange">
          <template slot="header">
            <h3 class="card-title title-up">Sign Up</h3>
          </template>

          <template>
            <div class="w-100 d-none pl-0 mt-4" :class="[success.response != '' ? 'd-block' : '']">
                <div class="alert alert-success pl-0 text-center">{{success.response}}</div>
            </div>

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.first_name[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.first_name[0]}}</div>
            </div>
            <fg-input
              class="no-border"
              v-model="first_name"
              placeholder="First Name..."
              addon-left-icon="now-ui-icons users_circle-08"
            >
            </fg-input>

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.last_name[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.last_name[0]}}</div>
            </div>
            <fg-input
              class="no-border"
              v-model="last_name"
              placeholder="Last Name..."
              addon-left-icon="now-ui-icons text_caps-small"
            >
            </fg-input>

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.email[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.email[0]}}</div>
            </div>
            <fg-input
              class="no-border"
              v-model="email"
              placeholder="Email"
              addon-left-icon="now-ui-icons ui-1_email-85"
            >
            </fg-input>

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.password[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.password[0]}}</div>
            </div>
            <fg-input
              type="password"
              class="no-border"
              v-model="password"
              placeholder="Password"
              addon-left-icon="now-ui-icons ui-user-08"
            >
            </fg-input>

            <div class="w-100 d-none pl-0 mt-4" :class="[errors.confirm_password[0] != '' ? 'd-block' : '']">
                <div class="alert alert-danger pl-0 text-center">{{errors.confirm_password[0]}}</div>
            </div>
            <fg-input
              type="password"
              class="no-border"
              v-model="confirm_password"
              placeholder="Confirm password"
              addon-left-icon="now-ui-icons ui-user-08"
            >
            </fg-input>

          </template>
          <div class="card-footer text-center">
            <n-button type="neutral" round @click="register" size="lg">Register</n-button>
          </div>
        </card>
      </div>

    </div>
  </div>
</template>
<script>
import axios from 'axios';
import { Card, FormGroupInput, Button } from '@/components';

export default {
  components: {
    Card,
    [Button.name]: Button,
    [FormGroupInput.name]: FormGroupInput
  },
  data() {
    return {
      last_name: "",
      first_name: "",
      email: "",
      password: "",
      confirm_password: "",
      errors: {
        first_name: [""],
        last_name: [""],
        email: [""],
        password: [""],
        confirm_password: [""],
        detail: [""]
      },
      success: {
        response: "",
      },
    };
  },
  mounted() {},
  methods: {
    clearErrorMessage() {
      this.success.response = "";
      for (const key in this.errors) {
        this.errors[key] = [""];
      }
    },
    register() {
      this.clearErrorMessage();
      const user = new FormData();
      const key = ["last_name", "first_name", "email", "password", "confirm_password"];
      const value = [this.last_name, this.first_name, this.email, this.password, this.confirm_password];

      console.log(value);
      for (let i = 0; i < value.length; i++) {
        user.append(key[i], value[i]);
      }

      console.log(user)
      // Post value
      axios
        .post("http://localhost:9000/user/", user)
        .then((response) => {
          this.success.response = response.data.response;
          setTimeout(() => {
            this.$router.push("/login");
          }, 3000)
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
  },
};
</script>
<style></style>
