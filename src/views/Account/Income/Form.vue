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
                  <v-ons-icon icon="md-calendar-check" class="list-item__icon text__color"></v-ons-icon>
                </div>
                <label class="center">
									<v-ons-input style="width: 90%" type="date" v-model="form.date">
									</v-ons-input>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon v-if="form.name != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                  <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-input type="text" placeholder="Income Name" v-model="form.name" float style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                    <v-ons-text class="text__danger" v-if="error.name">{{ error.name[0] }}
                    </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-show="!addInHead">
                <div class="left">
									<v-ons-icon v-if="form.in_head_id != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                  <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                  <select class="form-control" style="width: 90%" v-model="form.in_head_id">
										<option value="" disabled selected><label>ေငြစာရင္းေခါင္းစဥ္ ...</label></option>
                    <option v-for="inhead in inheads" :value="inhead.id">
                      {{ inhead.name }}
                    </option>
                  </select>
                </label>
								<div class="right">
									<v-ons-icon size="30px" icon="md-plus-square" class="list-item__icon text__color" @click="newInHead"></v-ons-icon>
								</div>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-show="addInHead">
                <div class="left">
									<v-ons-icon v-if="inhead.name != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                  <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                  <v-ons-input type="text" placeholder="Add Account Head" float style="width:90%;font-family:Zawgyi-One;" v-focus="inheadfocus" @focus="inheadfocus = true" @blur="inheadfocus = false" v-model="inhead.name"> </v-ons-input>
                  <v-ons-text class="text__danger" v-if="error.name">{{ error.name[0] }}
                  </v-ons-text>
                </label>
								<div class="right">
									<v-ons-icon size="30px" icon="md-check-square" class="list-item__icon text__color" @click="saveInHead"></v-ons-icon>
								</div>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon v-if="form.amount != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                  <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-input type="number" placeholder="Amount (number only)" v-model="form.amount" float style="width:90%"> </v-ons-input>
                    <v-ons-text class="text__danger" v-if="error.amount">{{ error.amount[0] }}
                    </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon v-if="form.description != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                  <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-input placeholder="Description" v-model="form.description" float type="textarea" style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                    <v-ons-text class="text__danger" v-if="error.description">{{ error.description[0] }}
                    </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
                <div class="left">
									<v-ons-icon v-if="form.remark != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                  <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                    <v-ons-input placeholder="Remark" type="text" v-model="form.remark" float style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                    <v-ons-text class="text__danger" v-if="error.remark">{{ error.remark[0] }}
                    </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''">
								<image-upload v-model="form.image" :forderPath="forderPath" style="width:95%; padding-left:5px"></image-upload>
								<v-ons-text class="text__danger" v-if="error.image">{{ error.image[0] }}
								</v-ons-text>
              </v-ons-list-item>
        </v-ons-list>
				<v-ons-modal
		      :visible="modalVisible"
		    >
		      <p style="text-align: center">
		        <!-- Save <v-ons-icon icon="fa-spinner" spin></v-ons-icon> -->
			      <v-ons-progress-circular indeterminate></v-ons-progress-circular>
		      </p>
		    </v-ons-modal>
	</v-ons-page>

</template>
<script type="text/javascript">
	import Vue from 'vue'
	// import Flash from '../../../helpers/flash'
	import Auth from '../../../store/auth'
	import { focus } from 'vue-focus';
	import { get, post, apiDomain } from '../../../helpers/api'
	import { toMulipartedForm } from '../../../helpers/form'
	import ImageUpload from '../../../components/ImageUpload.vue'
	import MonthShow from '../Month/Show.vue'

	export default {
		directives: { focus: focus },
		components: { ImageUpload, MonthShow },
		data() {
			return {
				authState: Auth.state,
				form: {},
				create: {},
				forderPath: 'vouchers/incomes/',
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
				error: {},
				inheads: [],
				inhead: { name:'' },
				infocus: false,
				inheadfocus: false,
				addInHead: false,
				isProcessing: false,
				modalVisible: false,
				initializeURL: apiDomain + `/incomes/create`,
				storeURL: apiDomain + `/incomes`,
				storeInHeadURL: apiDomain + `/inheads`,
				action: 'Create'
			}
		},
		created() {
			if (this.meta === 'create') {
				get(apiDomain + `/${this.month.year_id}/months/${this.month.id}`)
					.then((res) => {
						Vue.set(this.$data, 'form', res.data.income)
						Vue.set(this.$data, 'inheads', res.data.inheads)
					})
				}
				if(this.meta === 'edit') {
					get(apiDomain + `/${this.month.year_id}/months/${this.month.id}`)
					.then((res) => {
						Vue.set(this.$data, 'inheads', res.data.inheads)
					})
					this.initializeURL = apiDomain + `/incomes/${this.income.id}/edit`
					this.storeURL = apiDomain + `/incomes/${this.income.id}?_method=PUT`
					this.action = 'Update'
					this.form = this.income
				}
		},
		methods: {
			save() {
				this.form.month_id = this.month.id
				this.form.user_id = this.authState.user_id
				this.modalVisible = true
				const form = toMulipartedForm(this.form, this.meta)
				post(this.storeURL, form)
				    .then((res) => {
								const apiUrl = apiDomain
				        if(res.data.saved) {
									this.$store.commit('navigator/options', {
					          // Sets animations
					          animation: 'default',
					          // item: data,
					          // Resets default options
					          callback: () => this.$store.commit('navigator/options', {})
					        })
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
				        this.isProcessing = false
				    })
			},
			newInHead() {
				this.addInHead = !this.addInHead
				this.inheadfocus = true
			},
			saveInHead() {
				const inhead = toMulipartedForm(this.inhead, 'create')
				post(this.storeInHeadURL, inhead)
				.then((res) => {
					if(res.data.saved) {
						this.inheads.push(res.data.inhead)
						this.inhead.name = ''
						this.addInHead = false
					}
					this.isProcessing = false
				})
			},
		}
	}
</script>
