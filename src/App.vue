<template>
  <div id="app">
    <router-view/>
  </div>
</template>
<script>
export default {
  created() {
    if (
      localStorage.getItem("token") == "" &&
      localStorage.getItem("token") == null
    ) {
     this.$router.push('/login')
    }

  },
};
</script>
