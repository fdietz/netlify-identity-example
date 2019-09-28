<template>
  <div>
    <form @submit.prevent="login()" v-if="isLogin">
      <h2>Log In:</h2>
      <ul>
        <li>
          <label>
            <span>Email:</span>
            <input type="text" v-model="loginCreds.email">
          </label>
        </li>
        <li>
          <label>
            <span>Password:</span>
            <input type="password" ref="passwordField" v-model="loginCreds.password">
          </label>
        </li>
      </ul>
      <button type="submit" class="submit-button">Login</button>
      <p class="sign-up-link">
        New to chopsticks?
        <span @click="toggleSignup">Create an account.</span>
      </p>
    </form>
    <form @submit.prevent="signup()" v-else>
      <h2>Sign Up:</h2>
      <ul>
        <li>
          <label for>
            Email:
            <input type="text" v-model="signupCreds.email">
          </label>
        </li>
        <li>
          <label for>
            <span>Password:</span>
            <input type="password" ref="passwordField" v-model="signupCreds.password">
          </label>
        </li>
      </ul>
      <button type="submit" class="submit-button">Sign Me Up!</button>
      <p class="sign-up-link">
        Already have a log in?
        <span @click="toggleSignup">Return to log in.</span>
      </p>
    </form>
  </div>
</template>

<script>
import { mapState, mapActions } from "vuex";

export default {
  data() {
    return {
      auth: null,
      isLogin: true,
      showPassword: false,
      loginCreds: {
        email: null,
        password: null
      },
      signupCreds: {
        email: null,
        password: null
      }
    };
  },
  computed: {
    ...mapState("auth", ["currentUser"]),
  },
  methods: {
    ...mapActions("auth", ["attemptLogin", "attemptSignUp"]),
    toggleSignup() {
      this.isLogin = !this.isLogin;
    },
    signup() {
      this.attemptSignUp(this.signupCreds)
        .then(() => 
          console.log("You have successfully signed up. Check your email to confirm your account.")
        )
        .catch(err => this.handleUnsuccessfulLogin(err));
    },
    login() {
      let token = decodeURIComponent(window.location.search)
        .substring(1)
        .split("confirmation_token=")[1];
      this.attemptLogin({ token, ...this.loginCreds })
        .then(() => {
          this.handleSuccessfulLogin();
        })
        .catch(err => {
          this.handleUnsuccessfulLogin(err);
        });
    },
    handleSuccessfulLogin() {
      this.transferToDashboard();
      console.log("You have successfully logged in.")
    },
    handleUnsuccessfulLogin(err) {
      console.log("Oops! Looks like something is wrong!", err)
    },
    transferToDashboard() {
      this.$router.push(this.$route.query.redirect || "/");
    }
  }  
}
</script>

<style lang="scss" scoped>

</style>