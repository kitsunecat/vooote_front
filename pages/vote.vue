<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-text>
          <v-container>
            <div v-for="user in users" :key="user.id">
              <v-row>
                <v-col cols="3" sm="3" md="3">
                  <v-btn color="primary" v-on:click="voteUser(user)">{{user.name}}</v-btn>
                </v-col>
                <v-col cols="1" sm="1" md="1">
                  <span>{{user.number}}</span>
                </v-col>
                <v-col cols="7" sm="7" md="7">
                  <div  class="brown darken-2 text-center" :style="style(user.number)">
                    <span class="white--text">{{user.number}}</span>
                  </div>
                </v-col>
              </v-row>
            </div>
          </v-container>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>

export default {
  data: function () {
    return {
      newUser: "",
      users: []
    }
  },
  mounted() {
    this.fetchUsers()
  },
  methods: {
    async fetchUsers() {
      await this.$axios.$get('/dev/vooote')
      .then(response => {
        this.users = response.sort(i => i.id)
      })
    },
    async voteUser(user) {
      await this.fetchUsers;
      const url = '/dev/vooote/' + user.id
      await this.$axios.$put(url,{'name': user.name, 'number': Number(user.number + 1)}).then(response => {
        const index = this.users.findIndex(user => user.id === response.id)
        this.users.splice(index, 1, response);
      })
    },
    style(width) {
      return 'width:' + width + 'px'
    }
  }
}
</script>
