<template>
  <v-container>
    <v-row>
      <v-col>
        <h1 class="subtitle-1 dashboard grey--text">Dashboard</h1>
      </v-col>
    </v-row>
    <v-row class="mb-3">
      <v-tooltip top>
        <template v-slot:activator="{ on }">
          <v-btn text :class="isTitleActive ? 'active' : 'inactive'" @click="sortBy('title')" v-on="on">
            <v-icon text small left>mdi-folder</v-icon>
            <span class="text-capitalize">by project name</span>
          </v-btn>
        </template>
        <span>Sort alphabetically by project name</span>
      </v-tooltip>
      <v-tooltip top>
        <template v-slot:activator="{ on }">
          <v-btn text :class="isPersonActive ? 'active' : 'inactive'" @click="sortBy('person')" v-on="on">
            <v-icon text small left>mdi-account</v-icon>
            <span class="text-capitalize">by person</span>
          </v-btn>
        </template>
        <span>Sort alphabetically by person</span>
      </v-tooltip>
    </v-row>
    <v-card flat class="px-3" v-for="project in projects" :key="project.id">
      <v-row wrap :class="`px-3 project ${project.status}`">
        <v-col class="col-12 col-sm-12 col-md-6">
          <div class="project-title grey--text">Project name</div>
          <div class="project-content">{{ project.title }}</div>
        </v-col>
        <v-col class="col-12 col-sm-4 col-md-2">
          <div class="project-title grey--text">Person</div>
          <div class="project-content">{{ project.person }}</div>
        </v-col>
        <v-col class="col-12 col-sm-4 col-md-2">
          <div class="project-title grey--text">Due by</div>
          <div class="project-content">{{ new Date(project.due).toDateString() }}</div>
        </v-col>
        <v-col class="col-12 col-sm-4 col-md-2">
          <div class="project-content right">
            <v-chip small :class="project.status + ' white--text caption my-2'">{{ project.status }}</v-chip>
          </div>
        </v-col>
      </v-row>
      <v-divider></v-divider>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Dashboard',
  data () {
    return {
      projects: null,
      isTitleActive: false,
      isPersonActive: false
    }
  },
  methods: {
    populateProjects: function () {
      this.projects = null
      axios.get('http://127.0.0.1:3000/api/projects')
        .then(res => {
          this.projects = res.data
        })
        .catch(err => console.log(err))
    },
    sortBy: function (parameter) {
      this.projects.sort((a, b) => a[parameter] > b[parameter] ? 1 : -1)
      if (parameter === 'title') {
        this.isTitleActive = true
        this.isPersonActive = false
      } else if (parameter === 'person') {
        this.isTitleActive = false
        this.isPersonActive = true
      }
    }
  },
  mounted () {
    this.populateProjects()
  }
}
</script>

<style lang="scss" scoped>
  .project.ongoing {
    border-left: 4px solid #fed330;
  }
  .project.completed {
    border-left: 4px solid #26de81;
  }
  .project.overdue {
    border-left: 4px solid #fc5c65;
  }
  .v-chip.ongoing {
    background: #fed330 !important;
  }
  .v-chip.completed {
    background: #26de81 !important;
  }
  .v-chip.overdue {
    background: #fc5c65 !important;
  }
  .active {
    color: #45aaf2;
  }
  .inactive {
    color: grey;
  }
</style>
