<template>
  <div class="home">
    <HeaderNavigation />
    <div class="mt-3 w-50 mx-auto">
      <div class="w-50 mx-auto">
        <img
          class="w-100"
          alt="Vue logo"
          src="../assets/welcome-png-clipart-1.png"
        />
      </div>

      <div class="text-center">
        <span>Selamat Datang di Perpustakaan Online</span>
      </div>
    </div>

    <FooterPage />
  </div>
</template>

<script>
import HeaderNavigation from "@/components/HeaderNav.vue";
import FooterPage from "@/components/FooterPage.vue";
export default {
  name: "HomeView",
  components: { HeaderNavigation, FooterPage },
};
</script>
