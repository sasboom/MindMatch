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
      <b-button-group vertical class="formBtns">
        <b-button type="submit" variant="outline-success" id="submitBtn">
          <b-spinner small v-if="loading"></b-spinner>
          <span v-if="!loading" class="btnText">{{submitButtonText}}</span>
        </b-button>
        <b-button
          v-if="includeClearButton"
          @click="clearForm()"
          variant="outline-danger"
          id="clearBtn"
        >
          <span class="btnText">Clear</span>
        </b-button>
      </b-button-group>
    </b-form>
  </div>
</template>

<script>
import FormInput from "./FormInput.vue";
export default {
  name: "Form",
  props: {
    responseProp: String,
    formElements: Array,
    mutation: Object,
    includeClearButton: Boolean,
    submitButtonText: String
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
              response: data[this.responseProp],
              error: data[this.responseProp].error
            });
          }
        })
        .catch(err => {
          this.loading = false;
          this.$emit("formResponse", { response: err, error: true });
        });
    },
    clearForm() {
      this.form = {};
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
.formBtns {
  margin-top: 2vh;
  width: 100%;
}
.btnText {
  font-family: "BioRhyme";
  font-size: 15pt;
}
#submitBtn {
  margin-top: 1vh;
}
#clearBtn {
  margin-top: 1vh;
}
</style>
