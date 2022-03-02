<script setup>
defineProps({
  logo: {
    type: String,
    required: true,
  },
  alt: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <nav>
    <RouterLink to="/" id="logo_url">
      <img :src="logo" :alt="alt" id="logo" />
    </RouterLink>
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/pedidos">Pedidos</RouterLink>
  </nav>
</template>

<style scoped>
nav {
  background-color: #222;
  border-bottom: 4px solid #111;
  padding: 15px 50px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}
#logo_url {
  margin: auto;
  margin-left: 0;
}
#logo {
  height: 40px;
  width: 40px;
}
nav a {
  color: #fcba03;
  text-decoration: none;
  margin: 12px;
  transition: 0.5s;
}

nav a:hover {
  color: #fff;
}
</style>