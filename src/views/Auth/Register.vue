<template>
  	<v-ons-page>
  		  <custom-toolbar backLabel="Home" :title="'User Register'">
  				<template slot="right" v-if="form.password === form.password_confirmation">
  					<v-ons-icon style="color:white" icon="md-check" :disabled="isProcessing" @click="register"></v-ons-icon>
  			  </template>
  		  </custom-toolbar>
          <v-ons-list>
                <v-ons-list-header>
                <div class="info text-center">
                  <h4>Wan Yu</h4>
                </div>
                </v-ons-list-header>

                <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                  <div class="left">
  									<v-ons-icon v-if="form.name != ''" icon="md-label-alt" class="list-item__icon"  style="color: green;"></v-ons-icon>
                    <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                  </div>
                  <label class="center">
                      <v-ons-input placeholder="User Name" v-model="form.name" float type="text" style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                      <v-ons-text class="text__danger" v-if="error.name">{{ error.name[0] }}
                      </v-ons-text>
                  </label>
                </v-ons-list-item>

                <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                  <div class="left">
  									<v-ons-icon v-if="form.email != ''" icon="md-label-alt" class="list-item__icon"  style="color: green;"></v-ons-icon>
                    <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                  </div>
                  <label class="center">
                      <v-ons-input placeholder="Email" v-model="form.email" float type="email" style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                      <v-ons-text class="text__danger" v-if="error.email">{{ error.email[0] }}
                      </v-ons-text>
                  </label>
                </v-ons-list-item>

                <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                  <div class="left">
  									<v-ons-icon v-if="form.password != ''" icon="md-label-alt" class="list-item__icon"  style="color: green;"></v-ons-icon>
                    <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                  </div>
                  <label class="center">
                      <v-ons-input placeholder="Password" v-model="form.password" float type="password" style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                      <v-ons-text class="text__danger" v-if="error.password">{{ error.password[0] }}
                      </v-ons-text>
                  </label>
                </v-ons-list-item>

                <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                  <div class="left">
  									<v-ons-icon v-if="(form.password === form.password_confirmation) && (form.password_confirmation != '')" icon="md-label-alt" class="list-item__icon"  style="color: green;"></v-ons-icon>
                    <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                  </div>
                  <label class="center">
                      <v-ons-input placeholder="Confirm Password" v-model="form.password_confirmation" float type="password" style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                      <v-ons-text class="text__danger" v-if="error.password_confirmation">{{ error.password_confirmation[0] }}
                      </v-ons-text>

                      <v-ons-text v-if="form.password_confirmation != ''">
                        <span class="text__danger" v-if="form.password != form.password_confirmation">Password does not match.</span>
                      </v-ons-text>
                  </label>
                </v-ons-list-item>
          </v-ons-list>
  	</v-ons-page>

</template>
<script type="text/javascript">
    // import Flash from '../../helpers/flash'
    import { post, apiDomain } from '../../helpers/api'
    export default {
        data() {
            return {
                form: {
                    name: '',
                    email: '',
                    password: '',
                    password_confirmation: ''
                },
                error: {},
                isProcessing: false
            }
        },
        // computed() {
        //   return
        // }
        methods: {
            register() {
                this.isProcessing = true
                this.error = {}
                post( apiDomain + '/register', this.form)
                    .then((res) => {
                        if(res.data.registered) {
                            // Flash.setSuccess('Congratulations! You have now successfully registered.')
                            // this.$router.push('/login')
                            this.$store.commit('navigator/pop')
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
