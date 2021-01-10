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
          </v-container>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

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
  methods: {
    async createUser() {
      console.log(this.newUser);
      await this.$axios.$post('https://vizzduwbk3.execute-api.ap-northeast-1.amazonaws.com/dev/vooote',{'name': this.newUser, 'number': 0}).then(response => {
        console.log(response)
      })
    },
  }
}
</script>
