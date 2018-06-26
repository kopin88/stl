<template>
	<v-ons-page>
		  <custom-toolbar backLabel="ေငြစာရင္း" :title="title">
					<template slot="right" v-if="authState.api_token && authState.user_id == expense.user_id">
						<v-ons-icon style="color:white; padding-right:10px" icon="md-edit" @click="expenseEdit(animation, expense, apiUrl, month)"></v-ons-icon>
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
									<v-ons-text style="width: 90%" type="date">{{ expense.date | moment('DD - MMM - YYYY') }}
								</v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-text v-text="expense.name" float style="width:90%"> </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-text type="number" v-text="expense.amount" float style="width:90%"> </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-text v-text="expense.description" float style="width:90%"> </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-text v-text="expense.remark" float style="width:90%"> </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item v-show="expense.image != null" :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
									<img :src="imgLink + expense.image" style="width: 100%">
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
	import ExpenseForm from './Form.vue'
	import MonthShow from '../Month/Show.vue'
	Vue.use(require('vue-moment'));
	export default {
		components: {
		  ExpenseForm
		},
		data() {
			return {
				authState: Auth.state,
				animation:'default',
				apiUrl: apiDomain,
	      imgLink: imgUrl + 'vouchers/expenses/',
				isRemoving: false,
				modalVisible: false
			}
		},
		methods: {
			expenseEdit(name, data, apiUrl, month) {
				this.$store.commit('navigator/options', {
					// Sets animations
					animation: name,
					expense: data,
					month: month,
					// Resets default options
					callback: () => this.$store.commit('navigator/options', {})
				});

				this.$store.commit('navigator/push', {
					extends: ExpenseForm,
					data() {
						return {
							animation: name,
							expense: data,
							month: month,
							title: "Edit Expense",
							meta: 'edit'
						}
					}
				});
			},
			remove() {
				this.modalVisible = true
				post(apiDomain + `/expenses/${this.expense.id}?_method=DELETE`)
					.then((res) => {
						const apiUrl = 'http://stl.wanyumm.com/api'
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
									this.segmentIndex = 2
									this.ReportShow = false
									this.IncomeShow = false
									this.ExpenseShow = true
								}
							})
						}
					})
			}
			// remove() {
			// 	this.isRemoving = false
			// 	del( apiDomain + `/expenses/${this.expense.id}`)
			// 		.then((res) => {
			// 			if(res.data.deleted) {
			// 				this.$store.commit('navigator/pop')
			// 			}
			// 		})
			// }
		}
	}
</script>
