<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="logout">
      <span>Log me out</span>
    </button>
  </div>
</template>

<script>
import { mapActions } from "vuex";

export default {
  props: {
    msg: String
  },
  computed: {
    ...mapActions("auth", ["attemptLogout"]),
  },
  methods: {
    logout() {
      this.attemptLogout()
        .then(() => {
          this.handleSuccessfulLogout();
        })
        .catch(err => {
          this.handleUnsuccessfulLogout(err);
        });
    },
    handleSuccessfulLogout() {
      this.transferToLoginScreen();
      console.log("You have been successfully logged out.");
    },
    handleUnsuccessfulLogout(err) {
      console.log("Oops! Looks like something is wrong!");
    },
    transferToLoginScreen() {
      this.$router.push(this.$route.query.redirect || "/login");
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
