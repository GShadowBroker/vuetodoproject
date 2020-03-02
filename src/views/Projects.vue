<template>
  <v-container>
    <v-row>
      <v-col>
        <div class="projects">
          <h1 class="subtitle-1 grey--text">This is the About page</h1>
        </div>
      </v-col>
    </v-row>
    <v-row class="my-4">
      <v-col>
        <v-expansion-panels>
          <v-expansion-panel class="px-4" v-for="project in projects" :key="project.id">
            <v-expansion-panel-header class="subtitle-1">{{ project.title }}</v-expansion-panel-header>
            <v-expansion-panel-content class="grey--text">
              <v-container class="font-weight-bold space-between">
                <v-row justify="space-between">
                  <span>Due by {{ project.due.slice(0,10) }}</span>
                  <v-spacer></v-spacer>
                  <v-btn small text right class="grey--text">
                    Edit
                  </v-btn>
                  <v-btn
                    small
                    text
                    right
                    class="grey--text"
                    :loading="loadingDelBtn"
                    @click="deleteItem(project.id)"
                  >
                    Delete
                  </v-btn>
                </v-row>
              </v-container>
              <div>{{ project.description }}</div>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Team',
  data () {
    return {
      projects: null,
      user: { name: 'GledysonFS', role: 'Web developer, pica das galáxias', avatar: '/avatar-6.png' },
      loadingDelBtn: false
    }
  },
  mounted () {
    this.populateProjects()
  },
  methods: {
    populateProjects: function () {
      this.projects = []
      axios.get(`http://127.0.0.1:3000/api/projects/${this.user.name}`)
        .then(res => {
          this.projects = res.data
        })
        .catch(err => console.log(err))
    },
    deleteItem: function (id) {
      this.loadingDelBtn = true
      axios.delete(`http://127.0.0.1:3000/api/projects/delete/${id}`)
        .then(res => {
          this.$root.$emit('deleted')
          this.loadingDelBtn = false
          this.populateProjects()
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

<style>

</style>
