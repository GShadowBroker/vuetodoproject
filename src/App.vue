<template>
  <v-app class="grey lighten-4">
    <Navbar v-on:posted="posted" />
    <Alert :alertMessage="alertMessage" :alert="alert" :type="type"></Alert>
    <v-content class="grey lighten-4">
      <router-view/>
    </v-content>
  </v-app>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import Alert from '@/components/Alert.vue'
export default {
  name: 'App',
  components: {
    Navbar,
    Alert
  },
  data () {
    return {
      alertMessage: '',
      alert: false,
      type: 'success'
    }
  },
  methods: {
    posted: function () {
      this.alert = true
      this.type = 'success'
      this.alertMessage = 'Project created successfully'
      setTimeout(() => {
        this.alert = false
      }, 5000)
    },
    errored: function () {
      this.alert = true
      this.alertMessage = 'There was an error creating the project'
      this.type = 'error'
      setTimeout(() => {
        this.alert = false
      }, 5000)
    },
    deleted: function () {
      this.alert = true
      this.alertMessage = 'Project deleted successfully'
      this.type = 'success'
      setTimeout(() => {
        this.alert = false
      }, 5000)
    }
  },
  mounted () {
    this.$root.$on('deleted', () => {
      this.deleted()
    })
  }
}
</script>
