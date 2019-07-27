<template>
  <v-app id="inspire">
    <v-content>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4>
            <v-card class="elevation-12">
              <v-toolbar color="primary" dark flat>
                <v-toolbar-title>Login form</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-tooltip bottom>
                  <template v-slot:activator="{ on }">
                    <v-btn :href="source" icon large target="_blank" v-on="on">
                      <v-icon>mdi-code-tags</v-icon>
                    </v-btn>
                  </template>
                  <span>Source</span>
                </v-tooltip>
                <v-tooltip right>
                  <template v-slot:activator="{ on }">
                    <v-btn
                      icon
                      large
                      href="https://codepen.io/johnjleider/pen/pMvGQO"
                      target="_blank"
                      v-on="on"
                    >
                      <v-icon>mdi-codepen</v-icon>
                    </v-btn>
                  </template>
                  <span>Codepen</span>
                </v-tooltip>
              </v-toolbar>
              <v-card-text>
                <v-form v-model="validator">
                  <v-text-field
                    label="E-mail"
                    prepend-icon="person"
                    v-model="email"
                    :rules="emailRules"
                  ></v-text-field>

                  <v-text-field
                    name="Password"
                    label="Password"
                    hint="At least 8 characters"
                    min="8"
                    v-model="password"
                    prepend-icon="lock"
                    @keydown='(event) => {if (event.key == "Enter") this.submit()}'
                    :append-icon="value ? 'visibility' : 'visibility_off'"
                    @click:append="() => (value = !value)"
                    :type="value ? 'password' : 'text'"
                    :rules="passwordRules"
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" @click="submit">Login</v-btn>
              </v-card-actions>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>
<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    email: '',
    value: true,
    validator: '',
    password: '',
    emailRules: [
      v => !!v || 'E-mail is required',
      v =>
        /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
        'E-mail must be valid'
    ],
    passwordRules: [
      v => v.length >= 8 || 'Password must be at lest 8 charactors'
    ]
  }),
  methods: {
    submit: function () {
      if (this.validator === true) {
        if (this.email === 'ksi@example.com' && this.password === '88888888') {
          this.$router.push('/home')
        } else {
          alert('wrong username or password')
        }
      } else {
        alert('Please correct the form')
      }
    }
  },
  components: {}
}
</script>
