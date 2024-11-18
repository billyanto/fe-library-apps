<template>
  <div class="login">
    <HeaderNav/>
    <div class="w-25 border rounded mt-4 mx-auto">

      <div class="p-3">
        <img
          alt="Lib logo"
          class="w-100 p-3"
          src="../assets/Petersburg Public Library System.jpeg"
        />
        <div class="mt-1">
          <form @submit="checkForm" id="loginForm">
            <div class="mt-1">
              <div class="form-group">
                <p class="text-start">Email Address</p>
                <input
                  type="text"
                  class="form-control"
                  id="email"
                  v-model="email"
                />
              </div>
              <span class="text-danger" v-if="errors.email">{{
                errors.email
              }}</span>
            </div>

            <div class="mt-1 mb-1">
              <span class="text-start">Password</span>
              <div class="input-group">
                <input
                  :type="showPassword ? 'text' : 'password'"
                  class="form-control"
                  id="password"
                  v-model="password"
                />
                <span
                  class="input-group-text"
                  @click="showPassword = !showPassword"
                >
                  <fa-icon v-if="showPassword" icon="eye" />
                  <fa-icon v-if="!showPassword" icon="eye-slash" />
                </span>
              </div>
              <span class="text-danger" v-if="errors.password">{{
                errors.password
              }}</span>
            </div>

            <button
              class="btn btn-primary w-100 mt-2"
              type="submit"
              :disabled="isSubmitting"
            >
              Sign in
            </button>
          </form>
        </div>
      </div>
    </div>
    <FooterPage/>
  </div>
</template>
<script>
import HeaderNav from "@/components/HeaderNav.vue"
import FooterPage from "@/components/FooterPage.vue"
import axios from "axios";
function validateEmail(email) {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

  return emailPattern.test(email);
}

export default {
  name: "LoginApp",
  data() {
    return {
      showPassword: false,
      errors: { email: null, password: null },
      email: null,
      password: null,
      isSubmitting: false,
    };
  },
components: { HeaderNav,FooterPage },
  methods: {
    checkForm: function (e) {
      this.isSubmitting = true;
      this.errors.email = null;

      this.errors.password = null;

      if (!this.password) {
        this.errors.password("password required.");
        this.isSubmitting = false;
      }
      if (!this.email) {
        this.errors.email = "email required.";
        this.isSubmitting = false;
      }
      if (this.email && !validateEmail(this.email)) {
        this.errors.email = "wrong email format.";
        this.isSubmitting = false;
      } else if (!this.errors.email && !this.errors.password ) {
        this.loginAction();
      }

      e.preventDefault();
    },
    loginAction() {
      let payload = {
        email: this.email,
        password: this.password,
      };
      axios
        .post("/login", payload)
        .then((response) => {
          localStorage.setItem("token", response.data.session.id);
          this.$toast.success('Login Success');
          this.$router.push('/home')
          return response;
        })
        .catch((error) => {
          console.log(error)
          this.isSubmitting = false;
           this.$toast.error('Login Error');
          // if (error.response.data.errors != undefined) {
          //   console.log(error)
          // }
 
          return error;
        });
    },
  },
};
</script>
<style>
.display-eye {
  cursor: pointer;
}
</style>