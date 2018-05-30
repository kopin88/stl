<template>
  <v-ons-page>
	    <custom-toolbar backLabel="Anim" :title="title">
        <template slot="right">
					<v-ons-icon style="color:white" icon="md-check" :disabled="isProcessing" @click="save"></v-ons-icon>
			  </template>
	    </custom-toolbar>
        <v-ons-list>
              <v-ons-list-header>Text input</v-ons-list-header>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
                  <v-ons-icon icon="md-face" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                  <v-ons-select style="width: 90%" v-model="form.name">
                    <option v-for="month in monthList">
                      {{ month }}
                    </option>
                  </v-ons-select>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
                  <v-ons-icon icon="fa-question-circle-o" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-input placeholder="Monthly report title" v-model="form.title" float style="width:90%;font-family:Zawgyi-one;"> </v-ons-input>
                    <v-ons-text class="text__danger" v-if="error.title">{{ error.title[0] }}
                    </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-if="form.month_balance_id != null">
                <div class="left">
                  <v-ons-icon icon="md-face" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                  <select class="form-control" style="width: 90%" v-model="form.month_balance_id">
                    <option v-for="month in months" :value="month.id">
                      {{ month.name }}
                    </option>
                  </select>
                </label>
              </v-ons-list-item v-else>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-else>
                <div class="left">
                  <v-ons-icon icon="fa-question-circle-o" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                  <v-ons-input type="number" placeholder="Opening Balance" v-model="form.open_balance" float style="width:90%"> </v-ons-input>
                  <v-ons-text class="text__danger" v-if="error.title">{{ error.title[0] }}
                  </v-ons-text>
                </label>
              </v-ons-list-item>
        </v-ons-list>
        <v-ons-modal
		      :visible="modalVisible"
		    >
		      <p style="text-align: center">
		        Save <v-ons-icon icon="fa-spinner" spin></v-ons-icon>
		      </p>
		    </v-ons-modal>
  </v-ons-page>
</template>

<script type="text/javascript">
	import Vue from 'vue'
	// import Flash from '../../../helpers/flash'
	import { get, post, apiDomain } from '../../../helpers/api'
	import { toMulipartedForm } from '../../../helpers/form'

	export default {
		data() {
			return {
				monthList: {
					'January': 'January',
					'February': 'February',
					'March': 'March',
					'April': 'April',
					'May': 'May',
					'June': 'June',
					'July': 'July',
					'August': 'August',
					'September': 'September',
					'Outober': 'Outober',
					'November': 'November',
					'December': 'December'
				},
				months: [],
				error: {},
				isProcessing: false,
        modalVisible: false,
				action: 'Create'
			}
		},
		created() {
			get(apiDomain + `/months/${this.form.year_id}/edit/${this.form.id}`)
				.then((res) => {
					Vue.set(this.$data, 'months', res.data.months)
				})
		},
		methods: {
			save() {
        this.modalVisible = true
				const form = toMulipartedForm(this.form, 'edit')
				post(apiDomain + `/months/${this.form.id}?_method=PUT`, form)
				    .then((res) => {
				        if(res.data.saved) {
                  this.$store.commit('navigator/pop');
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
