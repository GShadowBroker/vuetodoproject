<template>
  <v-container>
    <v-row>
      <v-col>
        <div class="team">
          <h1 class="subtitle-1 grey--text">Our Team</h1>
        </div>
      </v-col>
    </v-row>
    <v-row wrap>
      <v-col
      class="col-12 col-sm-6 col-md-4 col-lg-3"
      v-for="person in team"
      :key="person.name"
      align="center"
      justify="center">
        <v-card outlined class="text-center" flat max-width="230px">
          <v-avatar class="profile" size="100">
            <v-img :src="person.avatar" :alt="person.name" contain></v-img>
          </v-avatar>
          <v-card-text>
            <div class="headline">{{ person.name }}</div>
            <div class="grey--text">{{ person.role }}</div>
          </v-card-text>
          <v-card-actions>
            <v-btn text color="grey">
              <v-icon left small>mdi-message</v-icon>
              <span>Message</span>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Team',
  data () {
    return {
      team: [
        { name: 'The Net Ninja', role: 'Web developer', avatar: '/avatar-1.png' },
        { name: 'Ryu', role: 'Graphic designer', avatar: '/avatar-2.png' },
        { name: 'Chun Li', role: 'Web developer', avatar: '/avatar-3.png' },
        { name: 'Gouken', role: 'Social media maverick', avatar: '/avatar-4.png' },
        { name: 'Yoshi', role: 'Sales guru', avatar: '/avatar-5.png' },
        { name: 'GledysonFS', role: 'Web developer, pica das galáxias', avatar: '/avatar-6.png' }
      ]
    }
  }
}
</script>

<style>
  .something {
    border: solid red;
  }
</style>
