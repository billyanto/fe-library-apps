<template>
  <div class="d-flex p-2 w-100 bg-light pr-2 pl-2">
    <RouterLink class="text-dark h4 my-auto mr-auto" to="/">Perpustakaan Online</RouterLink>
    <div v-if="currentRouteName !== 'login'">
      <RouterLink class="my-auto ml-2" to="/data-buku">Data Buku</RouterLink>
      <RouterLink class="my-auto ml-2" to="/data-buku">Transaksi</RouterLink>
      <a class="ml-2 pointer my-auto" @click="logout">Logout</a>
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