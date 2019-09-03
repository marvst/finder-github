<template>
  <v-container grid-list-xl class="mt-5">
    <v-layout row wrap>
      <v-flex md6 xs12>
        <v-card class="pa-4">
          <v-form>
             <v-text-field v-model="username" :counter="10" label="Type a Github's username" required />
             <v-btn color="success" class="mt-4" @click="findUser(username)" block>Find</v-btn>
          </v-form>
        <v-card class="pa-4 mt-6" v-if="!this.userInfo == ''">
          <v-img class="white--text" height="130px" :src="this.userInfo.avatar_url">
                <v-card-title class="align-end fill-height">{{this.userInfo.name}}</v-card-title>
              </v-img>
              <v-card-text>
                <span>@{{this.userInfo.login}}</span><br>
                <span>{{this.userInfo.bio}}</span>
                </v-card-text>
              <v-card-actions>
                <v-btn depressed small color="green" style="color: white" @click="addUser()" block>Add</v-btn>
              </v-card-actions>
        </v-card>  
        </v-card>
      </v-flex>
      <!-- Second Layout -->
      <v-flex md6 xs12>
        <v-layout row wrap>
          <v-flex md6 v-for="(item, index) in userList" :key="index">
            <v-card class="mb-2">
              <v-img class="white--text" height="130px" :src="item.avatar_url">
                <v-card-title class="align-end fill-height">{{item.name}}</v-card-title>
              </v-img>
              <v-card-text>
                <span>@{{item.login}}</span><br>
                <v-chip class="ma-2 ml-0" color="green" text-color="white">
                  <v-avatar left class="green darken-4">{{item.public_repos}}</v-avatar>Public Repos
                </v-chip>
                </v-card-text>
              <v-card-actions>
                <router-link :to="{name: 'about', params: {login: item.login}}" style="text-decoration: none !important">
                  <v-btn text color="orange">
                        Details
                    </v-btn>
                </router-link>
                <v-btn text color="red" @click="removeUser(item.id)">Remove</v-btn>
              </v-card-actions>
            </v-card>    
          </v-flex>
          
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>

import axios from 'axios'

export default {
  components: {
    
  },
  data() {
    return {
      username: '',
      userList: [],
      userInfo: ''
    }
  },
  methods: {
    async findUser(username) {
      let info = await axios.get(`https://api.github.com/users/${username}`)
      this.username = ''

      if (info.data.bio == null) {
        info.data.bio = "There is no bio on this profile..."
      }

      this.userInfo = {
        id: Date.now(),
        login: info.data.login,
        name: info.data.name,
        bio: info.data.bio,
        avatar_url: info.data.avatar_url,
        public_repos: info.data.public_repos
      }
      console.log(this.userInfo)
    },
    addUser() {
      this.userList.push(this.userInfo)
      this.userInfo = ''
    },
    removeUser(id) {
      console.log(id)
      this.userList = this.userList.filter(e => e.id != id)
    }
  }
};
</script>
