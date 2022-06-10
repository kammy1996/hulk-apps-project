<template>
  <div>
    <b-alert
      class="position-fixed fixed-bottom m-0 rounded-0"
      fade
      top
      :variant="variant"
      v-model="isAlert"
      id="alert"
    >
      {{ msg }}
    </b-alert>
  </div>
</template>

<script>
export default {
  name: "Alert",
  props: {
    isAlert: {
      default: false,
    },
    msg: String,
    variant: String,
  },
};
</script>

<style lang="scss" scoped></style>
