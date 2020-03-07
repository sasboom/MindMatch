<template>
  <b-alert :show="show" :variant="variant" dismissible @dismissed="dismissedFn">{{ message }}</b-alert>
</template>

<script>
export default {
  name: "MessageAlert",
  props: {
    show: Boolean,
    variant: String,
    message: String,
    dismissedFn: Function
  }
};
</script>
