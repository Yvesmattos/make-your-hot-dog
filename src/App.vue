<script setup>
import { RouterLink, RouterView } from "vue-router";
import Navbar from "./components/NavbarComponent.vue";
import Footer from "./components/FooterComponent.vue";

const logo_src = "/img/logo.png";
const app_name = "Make Your Hot Dog";
</script>

<template>
  <Navbar :logo="logo_src" :alt="app_name" />
  <RouterView />
  <Footer />
</template>

<style>
* {
  font-family: Arial, Helvetica, sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
main {
  margin: 50px;
  min-height: 314px;
}

h1 {
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>