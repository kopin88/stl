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
                    <v-ons-input placeholder="Expense Name" v-model="form.name" float type="text" style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                    <v-ons-text class="text__danger" v-if="error.name">{{ error.name[0] }}
                    </v-ons-text>
                </label>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-show="!addOutHead">
                <div class="left">
									<v-ons-icon v-if="form.out_head_id != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                  <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
									<select class="form-control" style="width: 90%" v-model="form.out_head_id">
										<option value="" disabled selected><label>ေငြစာရင္းေခါင္းစဥ္ ...</label></option>
                    <option v-for="outhead in outheads" :value="outhead.id">
                      {{ outhead.name }}
                    </option>
                  </select>
                </label>
								<div class="right">
									<v-ons-icon size="30px" icon="md-plus-square" class="list-item__icon text__color" @click="newOutHead"></v-ons-icon>
								</div>
              </v-ons-list-item>
              <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-show="addOutHead">
                <div class="left">
									<v-ons-icon v-if="outhead.name != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
                  <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
                </div>
                <label class="center">
                  <v-ons-input type="text" placeholder="Add Account Head" float style="width:90%;font-family:Zawgyi-One;" v-focus="outheadfocus" @focus="outheadfocus = true" @blur="outheadfocus = false" v-model="outhead.name"> </v-ons-input>
                  <v-ons-text class="text__danger" v-if="error.name">{{ error.name[0] }}
                  </v-ons-text>
                </label>
								<div class="right">
									<v-ons-icon size="30px" icon="md-check-square" class="list-item__icon text__color" @click="saveOutHead"></v-ons-icon>
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
                    <v-ons-input type="textarea" placeholder="Description" v-model="form.description" float style="width:90%;font-family:Zawgyi-One"> </v-ons-input>
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
                    <v-ons-input type="text" placeholder="Remark" v-model="form.remark" float style="width:90%;font-family:Zawgyi-One"> </v-ons-input>
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
				form: {},
				authState: Auth.state,
				forderPath: 'vouchers/expenses/',
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
				outheads: [],
				outhead: { name:'' },
				outfocus: false,
				outheadfocus: false,
				addOutHead: false,
				isProcessing: false,
				modalVisible: false,
				initializeURL: apiDomain + `/expenses/create`,
				storeURL: apiDomain + `/expenses`,
				storeOutHeadURL: apiDomain + `/outheads`,
				action: 'Create'
			}
		},
		created() {
				if (this.meta === 'create') {
					get(apiDomain + `/${this.month.year_id}/months/${this.month.id}`)
					.then((res) => {
						Vue.set(this.$data, 'form', res.data.expense)
						Vue.set(this.$data, 'outheads', res.data.outheads)
					})
				}
				if(this.meta === 'edit') {
					get(apiDomain + `/${this.month.year_id}/months/${this.month.id}`)
					.then((res) => {
						Vue.set(this.$data, 'outheads', res.data.outheads)
					})
					this.initializeURL = apiDomain + `/expenses/${this.expense.id}/edit`
					this.storeURL = apiDomain + `/expenses/${this.expense.id}?_method=PUT`
					this.action = 'Update'
					this.form = this.expense
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
											this.segmentIndex = 2
										  this.ReportShow = false
										  this.IncomeShow = false
										  this.ExpenseShow = true
										}
									})
				        }
				        this.isProcessing = false
				    })
			},
			newOutHead() {
				this.addOutHead = !this.addOutHead
				this.outheadfocus = true
			},
			saveOutHead() {
				const outhead = toMulipartedForm(this.outhead, 'create')
				post(this.storeOutHeadURL, outhead)
				.then((res) => {
					if(res.data.saved) {
						this.outheads.push(res.data.outhead)
						this.outhead.name = ''
						this.addOutHead = false
					}
					this.isProcessing = false
				})
			},
		}
	}
</script>
