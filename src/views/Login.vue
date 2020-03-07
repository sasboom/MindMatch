<template>
  <div>
    <MessageAlert
      :show="alertProps.show"
      :variant="alertProps.variant"
      :message="alertProps.message"
    />
    <b-container id="login-container">
      <b-row>
        <b-col></b-col>
        <b-col>
          <p id="title">Login</p>
        </b-col>
        <b-col></b-col>
      </b-row>
      <Form :formElements="formElements" :mutation="mutation" @formResponse="getLoginResponse" />
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src
import Form from "../components/Form.vue";
import LoginMutation from "../graphql/Login.gql";
import MessageAlert from "../components/MessageAlert";
import { onLogin } from "../vue-apollo";
export default {
  name: "Home",
  components: {
    Form,
    MessageAlert
  },
  data() {
    return {
      mutation: LoginMutation,
      formElements: [
        {
          label: "Username",
          placeholderText: "Please enter your username",
          mutationProp: "username",
          inputType: "text"
        },
        {
          label: "Password",
          placeHolterText: "Please enter your password",
          mutationProp: "password",
          inputType: "password"
        }
      ],
      alertProps: {
        show: false,
        variant: "",
        message: ""
      }
    };
  },
  methods: {
    getLoginResponse(data) {
      this.alertProps.show = true;
      if (data.error) {
        this.alertProps.variant = "danger";
        this.alertProps.message = "Invalid credential";
      } else {
        onLogin(this.$apollo, data.response.LoginUser.payload);
        this.$router.push("/match-finder");
      }
    }
  }
};
</script>

<style scoped>
#title {
  font-family: "BioRhyme";
  font-size: 25pt;
}
#login-container {
  border: 1px solid black;
  border-radius: 5px;
  padding: 5vw;
  margin-top: 2vh;
  box-shadow: 5px 5px #888888;
}
</style>
