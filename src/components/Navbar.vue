<template>
  <b-navbar toggleable="lg" type="light" variant="light" class="border-bottom">
    <b-navbar-brand>
      <b-link to="/">
        <p class="navlinks" id="app-name">MindMatch</p>
      </b-link>
    </b-navbar-brand>
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav class="ml-auto">
        <b-nav-item v-if="!isLoggedIn">
          <b-link to="/login">
            <p class="navlinks">Login</p>
          </b-link>
        </b-nav-item>
        <b-nav-item v-if="!isLoggedIn">
          <b-link to="/register">
            <p class="navlinks">Register</p>
          </b-link>
        </b-nav-item>
        <b-nav-item v-if="isLoggedIn">
          <b-button @click="logout()" variant="outline-danger">Logout</b-button>
        </b-nav-item>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script>
import { onLogout } from "../vue-apollo";
export default {
  name: "Navbar",
  data() {
    return {
      isLoggedIn: typeof localStorage.getItem("token") === "string"
    };
  },
  methods: {
    logout: function() {
      onLogout(this.$apollo);
      this.$router.push("/login");
    }
  }
};
</script>

<style scoped>
.navlinks {
  color: black;
  font-family: "BioRhyme";
  opacity: 0.5;
}
li a:hover {
  text-decoration: none;
}
.navbar-brand a:hover {
  text-decoration: none;
}
.navlinks:hover {
  opacity: 1;
}
.navlinks:visited {
  color: black;
}
</style>
