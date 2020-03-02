<template>
  <nav class="navbar">
    <v-app-bar
    color="grey lighten-4"
    flat
    app
    elevate-on-scroll>
      <v-app-bar-nav-icon
      class="grey--text text--darken-2"
      @click="mini = !mini"
      left></v-app-bar-nav-icon>
      <v-title class="text-uppercase grey--text">
        <span class="font-weight-light">Todo</span>
        <span class="grey--text text--darken-2">Ninja</span>
      </v-title>
      <v-spacer></v-spacer>
      <Popup v-on:posted="posted" v-on:errored="errored" />
      <v-menu offset-y open-on-hover>
        <template v-slot:activator="{ on }">
          <v-btn v-on="on" text color="grey">
            <span>Menu</span>
            <v-icon right>mdi-chevron-down</v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item
          v-for="link in links"
          :key="link.route"
          :to="link.route">
            <v-list-item-title>
              <v-icon left>{{ link.icon }}</v-icon>
              <span>{{ link.text }}</span>
            </v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
      <v-btn text color="grey">
        <span>Sign Out</span>
        <v-icon right>mdi-exit-to-app</v-icon>
      </v-btn>
    </v-app-bar>
    <v-navigation-drawer
    v-model="drawer"
    app
    :mini-variant.sync="mini"
    permanent
    class="primary">
      <v-list dark>
        <v-list-item>
          <v-list-item-avatar size="28" left>
            <v-img :src="user.avatar" :alt="user.name"></v-img>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title v-text="user.name"></v-list-item-title>
          </v-list-item-content>
          <v-btn
          icon
          right
          @click="mini = !mini">
            <v-icon>mdi-chevron-left</v-icon>
          </v-btn>
        </v-list-item>
        <v-divider></v-divider>
        <v-list-item v-for="link in links" :key="link.text" :to="link.route">
          <v-list-item-icon>
            <v-icon>{{ link.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ link.text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </nav>
</template>

<script>
import Popup from '@/components/Popup.vue'
export default {
  name: 'Navbar',
  components: { Popup },
  data () {
    return {
      drawer: false,
      links: [
        { icon: 'mdi-view-dashboard', text: 'Dashboard', route: '/' },
        { icon: 'mdi-folder', text: 'My Projects', route: '/projects' },
        { icon: 'mdi-account-group', text: 'Team', route: '/team' }
      ],
      mini: true,
      user: { name: 'GledysonFS', role: 'Web developer, pica das galáxias', avatar: '/avatar-6.png' }
    }
  },
  methods: {
    posted: function () {
      this.$emit('posted')
    },
    errored: function () {
      this.$emit('errored')
    }
  }
}
</script>

<style>
</style>
