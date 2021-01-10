<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-text>
          <v-container>
            <v-row>
              <h3>名前</h3>
            </v-row>
            <v-row>
              <v-col cols="9" sm="9" md="9">
                <v-text-field v-model="newUser" />
              </v-col>
              <v-col cols="3" sm="3" md="3">
                <v-btn v-on:click='createUser'>追加</v-btn>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="3" sm="3" md="3">
                <v-btn v-on:click='fetchUsers'>更新</v-btn>
              </v-col>
            </v-row>
            <div v-for="user in users" :key="user.id">
              <v-row>
                <v-col cols="5" sm="5" md="5">
                  <v-text-field v-model="user.name" />
                </v-col>
                <v-col cols="3" sm="3" md="3">
                  <v-text-field v-model="user.number" />
                </v-col>
                <v-col cols="2" sm="2" md="2">
                  <v-btn v-on:click="editUser(user)">編集</v-btn>
                </v-col>
                <v-col cols="2" sm="2" md="2">
                  <v-btn v-on:click="deleteUser(user)">削除</v-btn>
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
  // POST - https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote
  // GET - https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote
  // GET - https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote/{id}
  // PUT - https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote/{id}
  // DELETE - https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote/{id}

export default {
  data: function () {
    return {
      newUser: "",
      users: []
    }
  },
  created() {
    setInterval(() => {
      this.fetchUsers()
    }, 1000)
  },
  methods: {
    async fetchUsers() {
      await this.$axios.$get('https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote')
      .then(response => {
        this.users = response.sort(i => i.id)
      })
    },
    async createUser() {
      await this.$axios.$post('https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote',{'name': this.newUser, 'number': 0})
      .then(response => {
        this.users.push(response);
      })
    },
    async editUser(user) {
      const url = 'https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote/' + user.id
      await this.$axios.$put(url,{'name': user.name, 'number': user.number}).then(response => {
        const index = this.users.findIndex(user => user.id === response.id)
        this.users.splice(index, 1, response);
      })
    },
    async deleteUser(user) {
      const url = '/dev/vooote/' + user.id
      await this.$axios.$delete(url).then(response => {
        const index = this.users.findIndex(user => user.id === response.id)
        this.users.splice(index, 1);
      })
    },
  }
}
</script>
