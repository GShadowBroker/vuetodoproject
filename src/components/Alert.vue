<template>
  <div class="alert">
    <v-alert
      v-model="alert"
      :type="type"
      dismissible
      close-text="Close Alert"
      transition="slide-y-transition"
    >
      {{ alertMessage }}
    </v-alert>
  </div>
</template>

<script>
export default {
  name: 'Alert',
  props: {
    alertMessage: String,
    alert: Boolean,
    type: String
  },
  data () {
    return {
    }
  }
}
</script>

<style>
  .alert {
    position: fixed;
    top: 40px;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 100;
  }
</style>
