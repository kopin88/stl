<template>
	<v-ons-page>
		  <custom-toolbar backLabel="Anim" :title="title">
				<template slot="right" v-if="authState.api_token && authState.user_id == income.user_id">
					<v-ons-icon style="color:white; padding-right:10px" icon="md-edit" @click="incomeEdit(animation, income, apiUrl, month)"></v-ons-icon>
					<v-ons-icon style="color:white" icon="md-delete"  @click="remove" :disabled="isRemoving"></v-ons-icon>
				</template>
		  </custom-toolbar>
        <v-ons-list>
              <v-ons-list-header>Text input</v-ons-list-header>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
                  <v-ons-icon icon="md-calendar-check" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
									<v-ons-text style="width: 90%" type="date">{{income.date | moment('DD - MMM - YYYY')}}
								</v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-text v-text="income.name" float style="width:90%"> </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-text type="number" v-text="income.amount" float style="width:90%"> </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-text v-text="income.description" float style="width:90%"> </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-text v-text="income.remark" float style="width:90%"> </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item v-show="income.image != null" :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
									<img :src="imgLink + income.image" style="width: 100%">
                </label>
              </v-ons-list-item>
        </v-ons-list>
				<v-ons-modal
		      :visible="modalVisible"
		    >
		      <p style="text-align: center">
		        Delete <v-ons-icon icon="fa-spinner" spin></v-ons-icon>
		      </p>
		    </v-ons-modal>
	</v-ons-page>

</template>
<script type="text/javascript">
	import Vue from 'vue'
	import Auth from '../../../store/auth'
	import { imgUrl, apiDomain, post } from '../../../helpers/api'
	import IncomeForm from './Form.vue'
	import MonthShow from '../Month/Show.vue'
	Vue.use(require('vue-moment'));
	export default {
		components: {
		  IncomeForm
		},
		data() {
			return {
				authState: Auth.state,
				animation:'default',
				apiUrl: apiDomain,
	      imgLink: imgUrl + 'vouchers/incomes/',
				isRemoving: false,
				modalVisible: false,
			}
		},
		methods: {
			incomeEdit(name, data, apiUrl, month) {
				this.$store.commit('navigator/options', {
					// Sets animations
					animation: name,
					income: data,
					month: month,
					// Resets default options
					callback: () => this.$store.commit('navigator/options', {})
				});

				this.$store.commit('navigator/push', {
					extends: IncomeForm,
					data() {
						return {
							animation: name,
							income: data,
							month: month,
							title: "Edit Income",
							meta: 'edit'
						}
					}
				});
			},
			remove() {
				this.modalVisible = true
				post(apiDomain + `/incomes/${this.income.id}?_method=DELETE`)
					.then((res) => {
						const apiUrl = 'http://wanyumm.com/api'
						if(res.data.deleted) {
							this.$store.commit('navigator/replace', {
								extends: MonthShow,
								data() {
									return {
										month: res.data.month,
										title: res.data.month.name,
										reportsource: `months/${res.data.month.year_id}/reports/${res.data.month.id}`,
										insource: apiUrl + `/months/incomes/${res.data.month.id}`,
										outsource: apiUrl + `/months/expenses/${res.data.month.id}`,
									}
								},
								created() {
									this.segmentIndex = 1
									this.ReportShow = false
									this.IncomeShow = true
									this.ExpenseShow = false
								}
							})
						}
					})
			}
			// remove() {
			// 	this.isRemoving = false
			// 	del( apiDomain + `/incomes/${this.income.id}`)
			// 		.then((res) => {
			// 			if(res.data.deleted) {
			// 				this.$store.commit('navigator/pop')
			// 			}
			// 		})
			// }
		}
	}
</script>
