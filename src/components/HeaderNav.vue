<template>
  <div class="d-flex p-2 w-100 bg-light">
    <h4 class="me-auto pointer" href="#/home">Perpustakaan Online</h4>
    <div v-if="currentRouteName !== 'login'">
      <RouterLink to="/data-buku">Data Buku</RouterLink>
      <a class="ms-2 pointer" href="#/about">Transaksi</a>
      <a class="ms-2 pointer" @click="logout">Logout</a>
    </div>
    <div v-if="currentRouteName == 'login'">
      <a class="ms-2 pointer">Login</a>
    </div>
  </div>
</template>
<script>
export default {
  name: "HeaderNav",
  computed: {
    currentRouteName() {
      return this.$route.name;
    },
  },
  methods: {
    logout() {
      localStorage.setItem("token", "");
      this.$router.push("/login");
    },
  },
};
</script>
<style scoped>
.pointer {
  cursor: pointer;
}
</style>