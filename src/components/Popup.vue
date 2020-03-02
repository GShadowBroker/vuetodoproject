<template>
  <div class="popup text-center">
    <v-dialog v-model="dialog" max-width="600px">
      <template v-slot:activator="{ on }">
        <v-btn
          color="success lighten-1"
          dark
          v-on="on"
          text
          small
        >
          <v-icon left>mdi-plus</v-icon>
          <span>New project</span>
        </v-btn>
      </template>
      <v-card>
        <v-card-title
          class="headline"
          primary-title
        >
          <h3 class="grey--text">Add a new project</h3>
        </v-card-title>

        <v-card-text>
          <v-form class="px-4" v-model="valid" @change="enableSubmit" ref="form">
            <v-text-field
              v-model="title"
              :rules="titleRules"
              :counter="55"
              label="Title"
              clearable
              prepend-icon="mdi-folder"
              required
            ></v-text-field>
            <v-menu
              v-model="menu"
              :close-on-content-click="false"
              :nudge-right="40"
              transition="scale-transition"
              offset-y
              min-width="290px"
            >
              <template
                v-slot:activator="{ on }"
              >
                <v-text-field
                  label="Due by"
                  :value="formattedDate"
                  v-on="on"
                  readonly
                  prepend-icon="mdi-calendar"
                  required
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="due"
                @input="menu = false"
              ></v-date-picker>
            </v-menu>
            <v-textarea
              v-model="description"
              label="Description"
              :rules="descriptionRules"
              :counter="150"
              prepend-icon="mdi-lead-pencil"
              required
            ></v-textarea>
          </v-form>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn
            color="warning"
            text
            @click="$emit('posted')"
          >
            Test
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn
            color="grey"
            text
            @click="dialog = false"
          >
            Cancel
          </v-btn>
          <v-btn
            :disabled="!valid"
            color="success"
            text
            @click="submit"
            :loading="loadingBtn"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import format from 'date-fns/format'
import parseISO from 'date-fns/parseISO'
import axios from 'axios'
export default {
  name: 'Popup',
  data () {
    return {
      user: { name: 'GledysonFS', role: 'Web developer, pica das galáxias', avatar: '/avatar-6.png' },
      dialog: false,
      valid: false,
      title: '',
      titleRules: [
        v => !!v || 'Project name is required',
        v => v.length <= 55 || 'Project name must be less than 20 characters',
        v => v.length >= 2 || 'Project name is too short'
      ],
      due: '',
      dueRules: [
        v => !!v || 'Due date is required',
        v => new Date(v) > new Date() || 'Cannot be in the past'
      ],
      description: '',
      descriptionRules: [
        v => !!v || 'Description is required',
        v => v.length <= 150 || 'The description must be less than 150 characters',
        v => v.length >= 2 || 'Description is too short'
      ],
      menu: false,
      loadingBtn: false
    }
  },
  methods: {
    submit: function () {
      if (this.$refs.form.validate()) {
        this.loadingBtn = true
        const project = {
          title: this.title,
          due: this.due,
          description: this.description,
          person: this.user.name,
          status: 'ongoing'
        }

        axios.post('http://127.0.0.1:3000/api/projects/create', project)
          .then(res => {
            this.$emit('posted')
            this.dialog = false
            this.title = ''
            this.due = ''
            this.description = ''
            this.loadingBtn = false
          })
          .catch(err => {
            this.$emit('errored')
            console.log(err)
          })
      }
    },
    enableSubmit: function () {
      if (this.title.length > 0 && this.due.length > 0 && this.description.length > 0) {
        this.valid = true
      }
    }
  },
  computed: {
    formattedDate () {
      return this.due ? format(parseISO(this.due), 'do MMM yyyy') : ''
    }
  }
}
</script>

<style>
</style>
