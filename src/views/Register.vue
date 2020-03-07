<template>
  <div>
    <MessageAlert
      :show="alertProps.show"
      :variant="alertProps.variant"
      :message="alertProps.message"
    />
    <b-container id="register-container">
      <b-row>
        <b-col></b-col>
        <b-col id="title">Registration Form</b-col>
        <b-col></b-col>
      </b-row>
      <b-form @submit="createUser()" @submit.prevent>
        <b-form-group
          id="input-group-1"
          label="First Name:"
          label-for="first-name"
        >
          <b-form-input
            id="first-name"
            v-model="form.firstName"
            placeholder="Type a first name"
          ></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-2"
          label="Last Name:"
          label-for="last-name"
        >
          <b-form-input
            id="last-name"
            v-model="form.lastName"
            placeholder="Type a last name"
          ></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-3"
          label="Username:"
          label-for="username"
          description="This needs to be unique as you will be logging in with this."
        >
          <b-form-input
            id="username"
            v-model="form.username"
            placeholder="Type a username"
          ></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-4" label="Email:" label-for="email">
          <b-form-input
            id="email"
            v-model="form.email"
            placeholder="Type a email"
            type="email"
          ></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-5"
          label="Password:"
          label-for="password"
          description="This will not be shared with anyone."
        >
          <b-form-input
            id="password"
            v-model="form.password"
            placeholder="Type a strong password"
            type="password"
          ></b-form-input>
        </b-form-group>
        <b-button type="submit">
          <b-spinner small v-if="loading"></b-spinner>
          <span v-if="!loading">Submit</span>
        </b-button>
      </b-form>
    </b-container>
  </div>
</template>

<script>
const CREATE_USER = require("../graphql/CreateUser.gql");
import MessageAlert from "../components/MessageAlert";

export default {
  data() {
    return {
      form: {
        firstName: "",
        lastName: "",
        username: "",
        email: "",
        password: ""
      },
      alertProps: {
        show: false,
        variant: "danger",
        message: ""
      },
      loading: false
    };
  },
  components: {
    MessageAlert
  },
  methods: {
    createUser() {
      const resetAlertProps = () => {
        this.alertProps = {
          show: false,
          variant: "danger",
          message: ""
        };
      };
      const resetFormProps = () => {
        this.form = {
          firstName: "",
          lastName: "",
          username: "",
          email: "",
          password: ""
        };
      };
      resetAlertProps();
      this.loading = true;
      this.$apollo
        .mutate({
          mutation: CREATE_USER,
          variables: {
            input: {
              firstName: this.form.firstName,
              lastName: this.form.lastName,
              username: this.form.username,
              email: this.form.email,
              password: this.form.password
            }
          },
          update: () => {
            this.loading = false;
            this.alertProps.show = true;
            this.alertProps.variant = "success";
            this.alertProps.message =
              "Successfully created an account. Please login to continue.";
            resetFormProps();
          }
        })
        .catch(() => {
          this.loading = false;
          this.alertProps.variant = "danger";
          this.alertProps.show = true;
          this.alertProps.message = "An error occurred.";
        });
    }
  }
};
</script>

<style scoped>
#title {
  font-family: "BioRhyme";
  font-size: 15pt;
  margin-bottom: 3vh;
}

#register-container {
  border: 1px solid black;
  border-radius: 5px;
  padding: 5vw;
  margin-top: 2vh;
  box-shadow: 5px 5px #888888;
}
</style>
