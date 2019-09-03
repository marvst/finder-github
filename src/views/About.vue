<template>
  <div class="about">

    <v-container grid-list-xl class="mt-5">

        <v-layout row wrap>
          <v-flex md4 xs12 >
              <center>
                <v-avatar :size="200" class="mb-4">
                  <img :src="info.data.avatar_url" alt="avatar">
                </v-avatar>
                <h2 class="display-1">{{info.data.name}}</h2>
                <h2 class="subtitle-1">@{{info.data.login}}, {{info.data.location}}</h2>
                <h2 class="subtitle-2">{{info.data.bio}}</h2>
                 <v-chip class="ma-2 ml-0" color="green" text-color="white">
                  <v-avatar left class="green darken-4">{{info.data.public_repos}}</v-avatar>Public Repos
                </v-chip>
                <v-chip class="ma-2 ml-0" color="green" text-color="white">
                  <v-avatar left class="green darken-4">{{info.data.following}}</v-avatar>Following
                </v-chip>
                <v-chip class="ma-2 ml-0" color="green" text-color="white">
                  <v-avatar left class="green darken-4">{{info.data.followers}}</v-avatar>Followers
                </v-chip>
              </center>
          </v-flex>
          <!-- Second layout -->
          <v-flex md8 xs12 >
             <v-card class="pa-4">
               <h1 class="display-1">Public Repos</h1>
              <v-simple-table>
                  <thead>
                    <tr>
                      <th class="text-left">Name</th>
                      <th class="text-left">Stars</th>
                      <th class="text-left">Forks</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in reposList" :key="item.name">
                      <td class="pa-4">
                        <h3>{{ item.name }}</h3>
                        <h4 class="grey--text darken-4">{{item.language}}</h4>
                      </td>
                      <td>{{ item.stargazers_count }}</td>
                      <td>{{item.forks}}</td>
                    </tr>
                  </tbody>
                </v-simple-table>
             </v-card>
          </v-flex>
          
          
        </v-layout>
    </v-container>


    

  </div>
</template>


<script>

import axios from 'axios'

export default {
  components: {
    
  },
  data() {
    return {
      username: this.$route.params.login,
      info: '',
      reposList: []
    }
  },
  async mounted() {
    this.info = await axios.get(`https://api.github.com/users/${this.username}`)
    let repos = await axios.get(`https://api.github.com/users/${this.username}/repos`)
    this.reposList = repos.data
    this.username = ''
  },
  methods: {
    mostrar() {
      console.log(this.reposList)
    }
  }
};
</script>
