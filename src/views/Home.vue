<template>
  <b-container class="home" fluid>
    <b-jumbotron
      header="MindMatch"
      lead="Post your thoughts!"
      border-variant="dark"
      text-variant="dark"
    >
      <p>
        Welcome to MindMatch; a place to match with someone and share your ideas
        and thoughts
      </p>
      <p>To get started please either</p>
      <b-link to="/register">
        <b-button variant="outline-info" class="actionBtns">Register</b-button>
      </b-link>
      <b-link to="/login">
        <b-button variant="outline-success" class="actionBtns">Login</b-button>
      </b-link>
    </b-jumbotron>
  </b-container>
</template>

<script>
// @ is an alias to /src
export default {
  name: "Home"
};
</script>

<style scoped>
p {
  color: black;
}
.home {
  margin-top: 15vh;
  width: 100vw;
}
.actionBtns {
  margin: 1vh;
}
</style>
