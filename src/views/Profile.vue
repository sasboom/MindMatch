<template>
  <div>
    <MessageAlert
      :show="alertProps.show"
      :variant="alertProps.variant"
      :message="alertProps.message"
      :dismissedFn="resetAlertProps"
    />
    <b-container id="profile-container">
      <b-row>
        <b-col></b-col>
        <b-col>
          <p id="title">Profile</p>
        </b-col>
        <b-col></b-col>
      </b-row>
      <Form
        :formElements="formElements"
        :mutation="mutation"
        @formResponse="getCreateProfileResponse"
        :responseProp="responseProp"
        :includeClearButton="false"
        :additionalMutationData="additionalMutationData"
        submitButtonText="Create Profile"
      />
    </b-container>
  </div>
</template>

<script>
import CREATE_PROFILE from "../graphql/CreateProfile.gql";
import Form from "../components/Form.vue";
import MessageAlert from "../components/MessageAlert";

export default {
  data() {
    const user = JSON.parse(localStorage.getItem("user"));
    const additionalMutationData = {
      userId: user.id
    };
    return {
      mutation: CREATE_PROFILE,
      responseProp: "CreateProfile",
      formElements: [
        {
          label: "Bio",
          placeholderText: "Please enter your bio",
          mutationProp: "bio",
          inputType: "text"
        }
      ],
      additionalMutationData,
      alertProps: {
        show: false,
        variant: "",
        message: ""
      }
    };
  },
  components: {
    Form,
    MessageAlert
  },
  methods: {
    getCreateProfileResponse(data) {
      if (data.error) {
        this.alertProps.show = true;
        this.alertProps.variant = "danger";
        this.alertProps.message = "An error occurred";
      } else {
        this.alertProps.show = true;
        this.alertProps.variant = "success";
        this.alertProps.message =
          "Profile created!";
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

#profile-container {
  border: 1px solid black;
  border-radius: 5px;
  padding: 5vw;
  margin-top: 2vh;
  box-shadow: 5px 5px #888888;
}
</style>
