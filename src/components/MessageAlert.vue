<template>
  <b-alert :show="show" :variant="variant" dismissible>
    {{ message }}
  </b-alert>
</template>

<script>
export default {
  name: "MessageAlert",
  props: {
    show: Boolean,
    variant: String,
    message: String
  }
};
</script>
