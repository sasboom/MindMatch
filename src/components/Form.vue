<template>
  <div>
    <b-form @submit="submitForm()" @submit.prevent>
      <div v-for="(formElement, index) in formElements" :key="index">
        <FormInput
          :label="formElement.label"
          :placeHolderText="formElement.placeHolderText"
          :formInputId="formElement.mutationProp"
          :inputType="formElement.inputType"
          @formInputUpdated="getFormInputData"
          class="form-element"
        />
      </div>
      <b-button type="submit" id="submitBtn">
        <b-spinner small v-if="loading"></b-spinner>
        <span v-if="!loading">Submit</span>
      </b-button>
    </b-form>
  </div>
</template>

<script>
import FormInput from "./FormInput.vue";
export default {
  name: "Form",
  props: {
    formElements: Array,
    mutation: Object
  },
  data() {
    return {
      form: {},
      loading: false
    };
  },
  components: {
    FormInput
  },
  methods: {
    getFormInputData(data) {
      this.form[data.formInputId] = data.input;
    },
    submitForm() {
      this.loading = true;
      this.$apollo
        .mutate({
          mutation: this.mutation,
          variables: {
            input: this.form
          },
          update: (cache, { data }) => {
            this.loading = false;
            this.$emit("formResponse", {
              response: data,
              error: data.LoginUser.error
            });
          }
        })
        .catch(err => {
          this.loading = false;
          this.$emit("formResponse", { response: err, error: true });
        });
    }
  }
};
</script>

<style scoped>
.form-element {
  padding-top: 2vh;
  font-family: "BioRhyme";
  font-size: 15pt;
  text-align: left;
}
#submitBtn {
  margin-top: 2vh;
}
</style>
