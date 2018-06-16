<template>
  <v-ons-page>
     <v-ons-toolbar>
       <div class="center">Log In</div>
       <div class="right"><v-ons-toolbar-button>Close</v-ons-toolbar-button></div>
     </v-ons-toolbar>

     <div class="login-form">

       <input type="email" class="text-input--underbar" placeholder="Email" value="">
       <input type="password" class="text-input--underbar" placeholder="Password" value="">
       <br><br>
       <v-ons-button modifier="large" class="login-button">Log In</v-ons-button>
       <br><br>
       <v-ons-button modifier="quiet" class="forgot-password">Forgot password?</v-ons-button>
     </div>
   </v-ons-page>

  <!-- <v-v-ons-page>
    <v-v-ons-card>
        <img src="../../assets/stl-logo.png" alt="Wan Yu" style="width: 100%; padding:30px;">
      <div class="content">
        <v-v-ons-list>
          <v-v-ons-list-header>Login</v-v-ons-list-header>
            <v-v-ons-list-item>
              <div class="left">
                <v-v-ons-icon style="color:green" icon="ion-email" class="list-item__icon"></v-v-ons-icon>
              </div>
              <label class="center">
                <v-v-ons-input type="email" placeholder="Eamil" float v-model="form.email" style="width:100%">
                </v-v-ons-input>
              </label>
          </v-v-ons-list-item>
            <v-v-ons-list-item>
              <div class="left">
                <v-v-ons-icon style="color:green" icon="ion-android-lock" class="list-item__icon"></v-v-ons-icon>
              </div>
              <label class="center">
                <v-v-ons-input type="password" float placeholder="Password" style="width:100%" v-model="form.password">
                </v-v-ons-input>
              </label>
          </v-v-ons-list-item>
          <v-v-ons-list-item>
            <div class="left">

            </div>
            <label class="center">
              <v-ons-button @click="login" :disabled="isProcessing" modifier="cta" class="login-button">Log In</v-ons-button>
            </label>
          </v-v-ons-list-item>
        </v-v-ons-list>
      </div>
    </v-v-ons-card>
  </v-v-ons-page> -->
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
                        if(err.respv-onse.status === 422) {
                            this.error = err.respv-onse.data
                        }
                        this.isProcessing = false
                    })
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login-form {
  text-align: center;
  width: 80%;
  margin: 60px auto 0;
}

input[type=email], input[type=password] {
  display: block;
  width: 100%;
  margin: 0 auto;
  outline: none;
  height: 100%;
  padding-top: 5px;
  padding-bottom: 6px;
}

.login-button {
  width: 100%;
  margin: 0 auto;
}

.forgot-password {
  display: block;
  margin: 8px auto 0 auto;
  font-size: 14px;
}

</style>
