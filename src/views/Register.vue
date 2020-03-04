<template>
  <div>
    <b-alert v-model="error" variant="danger" dismissible>
      An error occurred
    </b-alert>
    <b-alert v-model="success" variant="success" dismissible>
      Successfully created account. Please log in to continue.
    </b-alert>
    <b-modal ref="success-modal" hide-footer title="Success!">
      <div class="d-block text-center">
        <h3>Your account was successfully created!</h3>
        <p>Please log in </p>
      </div>
      <b-button class="mt-3" variant="outline-danger" block @click="hideErrorModal">Close</b-button>
    </b-modal>
    <b-container v-if="!error" id="register-container">
      <b-row><b-col></b-col><b-col id="title">Registration Form</b-col><b-col></b-col></b-row>
      <b-form @submit="createUser()" @submit.prevent>
        <b-form-group
          id="input-group-1"
          label="First Name:"
          label-for="first-name"
        >
          <b-form-input
            id="first-name"
            v-model="firstName"
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
          v-model="lastName"
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
            v-model="username"
            placeholder="Type a username"
          ></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-1"
          label="Email:"
          label-for="email"
          description="This will not be shared with anyone."
        >
          <b-form-input
            id="email"
            v-model="email"
            placeholder="Type a email"
            type="email"
          ></b-form-input>
        </b-form-group>
        <b-button type="submit"><b-spinner small v-if="loading"></b-spinner><span v-if="!loading">Submit</span></b-button>
      </b-form>
    </b-container>
  </div>
</template>

<script>
const CREATE_USER = require('../graphql/CreateUser.gql');
export default {
  data () {
    return {
      firstName: '',
      lastName: '',
      username: '',
      email: '',
      password: '',
      loading: false,
      error: false,
      success: false,
    }
  },
  methods: {
    hideErrorModal() {
      this.$refs['error-modal'].hide()
    },
    createUser () {
      this.loading = true;
      this.$apollo.mutate({
        mutation: CREATE_USER,
        variables: {
          input: {
            firstName: this.firstName,
            lastName: this.lastName,
            username: this.username,
            email: this.email,
            password: this.password
          },
        },
        update: () => {
          this.loading = false;
          this.error = false;
          this.success = true;
        }
      })
      .catch(() => {
        this.loading = false;
        this.error = true;
        this.success = false;
      });
    }
  },
}
</script>

<style scoped>
#title {
  font-family: 'BioRhyme';
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