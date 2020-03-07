<template>
  <div id="app">
    <b-navbar toggleable="lg" type="light" variant="light" class="border-bottom">
      <b-navbar-brand>
        <b-link to="/">
          <p class="navlinks" id="app-name">MindMatch</p>
        </b-link>
      </b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav class="ml-auto">
          <b-nav-item>
            <b-link to="/login">
              <p class="navlinks">Login</p>
            </b-link>
          </b-nav-item>
          <b-nav-item>
            <b-link to="/register">
              <p class="navlinks">Register</p>
            </b-link>
          </b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <transition name="fade" mode="out-in">
      <router-view></router-view>
    </transition>
  </div>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
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
#app-name {
  opacity: 1;
}
</style>
