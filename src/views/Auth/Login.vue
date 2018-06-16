<template>
  <v-ons-page style="margin-top:40px">
    <!-- <v-ons-toolbar>
      <div class="center"></div>
    </v-ons-toolbar> -->

    <v-ons-card>
        <img src="../../assets/stl-logo.png" alt="Wan Yu" style="width: 100%; padding:30px;">
      <div class="content">
        <v-ons-list>
          <!-- <v-ons-list-header>Login</v-ons-list-header> -->
            <v-ons-list-item>
              <div class="left">
                <v-ons-icon icon="ion-email" class="list-item__icon" v-if="form.email == ''"></v-ons-icon>
                <v-ons-icon icon="ion-email" class="list-item__icon text__color" v-else></v-ons-icon>
              </div>
              <div class="center">
                <v-ons-input type="email" placeholder="Eamil" class="form__control" float v-model="form.email">
                </v-ons-input>
              </div>
          </v-ons-list-item>
            <v-ons-list-item>
              <div class="left">
                <v-ons-icon icon="ion-android-lock" class="list-item__icon" v-if="form.password == ''"></v-ons-icon>
                <v-ons-icon icon="ion-android-lock" class="list-item__icon text__color" v-else></v-ons-icon>
              </div>
              <div class="center">
                <v-ons-input type="password" float placeholder="Password" class="form__control" v-model="form.password">
                </v-ons-input>
              </div>
          </v-ons-list-item>
          <v-ons-list-item>
            <!-- <div class="left">

            </div>
            <label class="center">
            </label> -->
            <ons-button @click="login" :disabled="isProcessing" modifier="large" class="theme__color">Log In</ons-button>
          </v-ons-list-item>
        </v-ons-list>
      </div>
    </v-ons-card>
  </v-ons-page>
</template>

<script>
    import Auth from '../../store/auth'
    import { get, post, apiDomain } from '../../helpers/api'
    export default {
        name : "LogIn",
        data() {
            return {
                form: {
                    email: '',
                    password: ''
                },
                error: {},
                isProcessing: false
            }
        },
        methods: {
            login() {
                this.isProcessing = true
                // this.error = {}
                post( apiDomain + '/login', this.form)
                    .then((res) => {
                        if(res.data.authenticated) {
                            // set token
                            Auth.set(res.data.api_token, res.data.user_id)
                            // this.$router.push(/)
                        }
                        this.isProcessing = false
                    })
                    .catch((err) => {
                        if(err.response.status === 422) {
                            this.error = err.response.data
                        }
                        this.isProcessing = false
                    })
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.form__control {
  width: 100%;
  /* margin: 0 auto; */
}
.list-item--material__left {
  min-width: 30px;
}
.list-item {  
  min-height: 80px;
}

.forgot-password {
  display: block;
  margin: 8px auto 0 auto;
  font-size: 14px;
}

</style>
