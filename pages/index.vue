<template>
  <v-app id="inspire">
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-layout
          justify="center"
        >
          <v-flex offset-md3 md6
          >
            <v-card class="elevation-12">
              <v-toolbar
                color="primary"
                dark
                flat
              >
                <v-toolbar-title>Login form</v-toolbar-title>
                <v-spacer/>
                <v-tooltip bottom>
                  <template v-slot:activator="{ on }">
                    <v-btn
                      :href="source"
                      icon
                      large
                      target="_blank"
                      v-on="on"
                    >
                      <v-icon>mdi-code-tags</v-icon>
                    </v-btn>
                  </template>
                  <span>Source</span>
                </v-tooltip>
              </v-toolbar>
              <v-card-text>
                <template>
                  <v-form
                    ref="form"
                    v-model="valid"
                    lazy-validation
                  >
                    <v-text-field
                      v-model="email"
                      :rules="emailRules"
                      label="E-mail"
                      required
                    ></v-text-field>
                    <v-text-field
                      type="password"
                      v-model="password"
                      :counter="25"
                      :rules="passwordRules"
                      label="password"
                      required
                    ></v-text-field>
                    <v-btn
                      :disabled="!valid"
                      color="success"
                      @click="validate"
                    >
                      Validate
                    </v-btn>

                    <v-btn
                      color="error"
                      @click="reset"
                    >
                      Reset Form
                    </v-btn>

                    <v-btn
                      color="warning"
                      @click="resetValidation"
                    >
                      Reset Validation
                    </v-btn>
                  </v-form>
                </template>
              </v-card-text>
              <v-card-actions>
                <v-spacer/>
                <v-btn color="primary" @click="validate">Login</v-btn>
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
            source: String,
        },
        data: () => ({
            valid: true,
            password: '',
            passwordRules: [
                v => !!v || 'password is required',
                v => (v && v.length <= 25) || 'password must be less than 25 characters'
            ],
            email: '',
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+/.test(v) || 'E-mail must be valid'
            ],
        }),
        computed: {
            loginData() {
                return {
                    email: this.email,
                    password: this.password,
                }
            }
        },
        methods: {
            validate() {
                if (this.$refs.form.validate()) {
                    this.$axios.post('http://127.0.0.1:8000/api/auth/login', this.loginData)
                        .then(response => {
                            localStorage.setItem('token', response.data)
                            console.log(response.data)
                        })
                        .catch(error => Promise.reject(error))
                    this.$router.push('/brand')
                    //window.history.pushState("object or string", "Title", "/inspire");
                    //location.href='http://localhost:3000/staff-center'
                }
            },
            reset() {
                this.$refs.form.reset()
            },
            resetValidation() {
                this.$refs.form.resetValidation()
            }
        }
    }
</script>
