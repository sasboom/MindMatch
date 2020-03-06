<template>
  <b-form-group :id="formInputGroupId" :label="label" :label-for="formInputId">
    <b-form-input
      :id="formInputId"
      v-model="formInput"
      :placeholder="placeholderText"
      v-on:input="updateFormInput"
      :type="inputType"
    ></b-form-input>
  </b-form-group>
</template>

<script>
export default {
  name: "FormInput",
  data() {
    return {
      formInput: "",
      formInputGroupId: `fig-${Math.random().toString()}`
    };
  },
  props: {
    label: String,
    formInputId: String,
    placeholderText: String,
    inputType: String
  },
  methods: {
    updateFormInput(event) {
      this.formInput = event;
      this.$emit("formInputUpdated", {
        input: this.formInput,
        formInputId: this.formInputId
      });
    }
  }
};
</script>
