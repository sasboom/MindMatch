<template>
  <div>
    <MessageAlert
      :show="alertProps.show"
      :variant="alertProps.variant"
      :message="alertProps.message"
      :dismissedFn="resetAlertProps"
    />
    <b-container id="register-container">
      <b-row>
        <b-col></b-col>
        <b-col id="title">Registration Form</b-col>
        <b-col></b-col>
      </b-row>
      <Form
        :formElements="formElements"
        :mutation="mutation"
        @formResponse="getRegisterResponse"
        :responseProp="responseProp"
        :includeClearButton="true"
        submitButtonText="Register"
      />
    </b-container>
  </div>
</template>

<script>
const CREATE_USER = require("../graphql/CreateUser.gql");
import MessageAlert from "../components/MessageAlert";
import Form from "../components/Form.vue";

export default {
  data() {
    return {
      mutation: CREATE_USER,
      responseProp: "CreateUser",
      formElements: [
        {
          label: "First Name",
          placeholderText: "Please enter your first name",
          mutationProp: "firstName",
          inputType: "text"
        },
        {
          label: "Last Name",
          placeHolterText: "Please enter your last name",
          mutationProp: "lastName",
          inputType: "text"
        },
        {
          label: "Username",
          placeHolterText: "Please enter your username",
          mutationProp: "username",
          inputType: "text"
        },
        {
          label: "Email",
          placeHolterText: "Please enter your email",
          mutationProp: "email",
          inputType: "email"
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
  components: {
    MessageAlert,
    Form
  },
  methods: {
    getRegisterResponse(data) {
      if (data.error) {
        this.alertProps.show = true;
        this.alertProps.variant = "danger";
        this.alertProps.message = "An error occurred";
      } else {
        this.alertProps.show = true;
        this.alertProps.variant = "success";
        this.alertProps.message =
          "Successfully created an account. Please login to continue.";
      }
    },
    resetAlertProps() {
      this.alertProps = {
        show: false,
        variant: "",
        message: ""
      };
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
