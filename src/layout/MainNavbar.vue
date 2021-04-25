<template>
  <navbar
    position="fixed"
    type="primary"
    :transparent="transparent"
    :color-on-scroll="colorOnScroll"
    menu-classes="ml-auto"
  >
    <template>
      <router-link v-popover:popover1 class="navbar-brand" to="/home">
        Trade core blog
      </router-link>
    </template>
    <template slot="navbar-menu">
      <li class="nav-item">
        <router-link
          class="nav-link"
          to="/home"
        >
          <p>Home</p>
        </router-link>
      </li>
      <li class="nav-item">
        <router-link
          class="nav-link btn btn-neutral"
          to="/create"
        >
          <p>Create post</p>
        </router-link>
      </li>
      <li class="nav-item">
        <router-link
          class="nav-link"
          to="/login"
        >
          <p>Login</p>
        </router-link>
      </li>
    </template>
  </navbar>
</template>

<script>
import { DropDown, Navbar, NavLink } from '@/components';
import { Popover } from 'element-ui';
export default {
  name: 'main-navbar',
  props: {
    transparent: Boolean,
    colorOnScroll: Number
  },
  components: {
    Navbar,
    [Popover.name]: Popover
  }
};
</script>

<style scoped></style>
