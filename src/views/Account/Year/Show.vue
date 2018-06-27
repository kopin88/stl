<template>
  <v-ons-page>
	    <custom-toolbar backLabel="Anim" :title="title">
        <template slot="right">
          <v-ons-icon style="color:white; padding-right:10px" icon="md-edit"  @click="editFinancialYear = true"></v-ons-icon>
        </template>
	    </custom-toolbar>
				<v-ons-card>
			    <v-ons-row>
			      <v-ons-col width="35%"><v-ons-text>ဘ႑ာေရးႏွစ္ : </v-ons-text>
			      </v-ons-col>
			      <v-ons-col width="55%"><v-ons-text>{{ year.name }}</v-ons-text>
			      </v-ons-col>
			      <v-ons-col width="10%">
							<!-- <v-ons-icon style="color:green;" icon="md-edit" class="pull-right" @click="editFinancialYear = true"></v-ons-icon> -->
			      </v-ons-col>
			    </v-ons-row>
					<v-ons-row>
			      <v-ons-col><v-ons-text>စာရင္းဖြင့္ : </v-ons-text>
			      </v-ons-col>
			      <v-ons-col><v-ons-text class="pull-right">{{ year.open_balance }}</v-ons-text>
			      </v-ons-col>
						<v-ons-col>
			      </v-ons-col>
			    </v-ons-row>
			    <v-ons-row>
			      <v-ons-col><v-ons-text>လက္က်န္ : </v-ons-text>
			      </v-ons-col>
			      <v-ons-col><v-ons-text class="pull-right">{{ year.balance }}</v-ons-text>
			      </v-ons-col>
						<v-ons-col>
			      </v-ons-col>
			    </v-ons-row>
		      <v-ons-row style="margin-top:20px">
		        <div class="table-responsive">
		            <table class="table table-striped table-no-bordered table-hover dataTable dtr-inline" cellspacing="0" width="100%" style="width: 700px;" role="grid" aria-describedby="datatables_info">
		              <thead>
		                <tr>
		                  <th class="text-right">#</th>
		                  <th class="text-center">ေငြစာရင္း</th>
		                  <th class="text-center">စာရင္းဖြင့္</th>
		                  <th class="text-center">၀င္ေငြ</th>
		                  <th class="text-center">အသံုးစရိတ္</th>
		                  <th class="text-center">လက္က်န္</th>
		                  <th class="text-center">မွတ္ခ်က္</th>
		                </tr>
		              </thead>
		              <tbody>
		                <tr v-for="(month, index) in months" :key="month.id" @click="financialMonth(animation, month, apiUrl)">
		                  <td><v-ons-text>{{ index + 1 }}</v-ons-text></td>
		                  <td><v-ons-text>{{ month.name }}</v-ons-text></td>
		                  <td class="text-right"><v-ons-text>{{ month.openBalance }}</v-ons-text></td>
		                  <td class="text-right"><v-ons-text>{{ month.in_total }}</v-ons-text></td>
		                  <td class="text-right"><v-ons-text>{{ month.out_total }}</v-ons-text></td>
		                  <td class="text-right"><v-ons-text>{{ month.balance }}</v-ons-text></td>
		                  <td></td>
		                </tr>
		              </tbody>
                  <tfoot>
      							<tr>
      								<th></th>
      								<th>Total</th>
      								<th class="table-number text-warning" v-if="year.open_balance != OpenBalance">{{ year.open_balance }}</th>
      								<th class="table-number" v-else>{{year.open_balance}}</th>
      								<th class="table-number">{{ inTotal }}</th>
      								<th class="table-number">{{ outTotal }}</th>
      								<th class="table-number text-warning" v-if="year.balance != Balance">{{ year.balance }}</th>
      								<th class="table-number" v-else>{{year.balance}}</th>
      								<th class="text-center">
      									<v-ons-icon class="text-warning" icon="md-alert-circle"  v-if="year.balance != Balance" @click="saveBalance" :disabled="isProcessing"></v-ons-icon>
              					<v-ons-icon class="text__color" icon="md-check-circle" v-else></v-ons-icon>
              				</th>
      							</tr>
      						</tfoot>
		            </table>
		          </div>
		    	</v-ons-row>
				</v-ons-card>
        <v-ons-card>
          <v-ons-text>
            With Chart
          </v-ons-text><br>
          <!-- <div style="width:500px"> -->
            <template>
              <div class="table-responsive">
  		            <table class="table table-striped table-no-bordered table-hover dataTable dtr-inline" cellspacing="0" width="100%" style="width: 1500px; margin-bottom:80px" role="grid" aria-describedby="datatables_info">
                    <yearly
                      :year_id="year.id"
                      :height="300"
                      >
                    </yearly>
    		          </table>
              </div>
            </template>
          <!-- </div> -->
        </v-ons-card>

			      <v-ons-fab position="bottom right" @click="addMonth = true">
			        <v-ons-icon icon="md-plus"></v-ons-icon>
			      </v-ons-fab>

			      <v-ons-alert-dialog modifier="rowfooter" :visible.sync="addMonth">
			        <span slot="title">Month</span>
							<v-ons-row class="mg-top-10">
								<v-ons-col>
									<select class="form-control" v-model="form.name">
										<option value="" disabled selected><label>Select ...</label></option>
										<option v-for="month in monthList">
											{{month}}
										</option>
									</select>
								</v-ons-col>
							</v-ons-row>
							<v-ons-row class="mg-top-10">
								<v-ons-col>
									<v-ons-input type="text" placeholder="Title" v-model="form.title" float style="width:100%; font-family:Zawgyi-One"> </v-ons-input>
									<v-ons-text class="text__danger" v-if="error.title">{{ error.title[0] }}
									</v-ons-text>
								</v-ons-col>
							</v-ons-row>
							<v-ons-row class="mg-top-10">
								<v-ons-col>
									<div v-if="monthBalance">
										<select class="form-control" v-model="form.month_balance_id">
											<option value="" disabled selected>
												<label>Opening Balance </label>
											</option>
											<option v-for="month in months" :value="month.id">
												{{month.name}}
											</option>
										</select>
									</div>
									<div v-else>
										<v-ons-input type="number" v-model="form.open_balance" float style="width:100%" placeholder="Opening Balance">
										</v-ons-input>
										<v-ons-text class="text__danger" v-if="error.open_balance">{{ error.open_balance[0] }}
										</v-ons-text>
									</div>
								</v-ons-col>
							</v-ons-row>
			        <template slot="footer">
			          <div class="alert-dialog-button" @click="addMonth = false">Cancel</div>
			          <div class="alert-dialog-button"  @click="save" :disabled="isProcessing">Save</div>
			        </template>
			      </v-ons-alert-dialog>
						<v-ons-alert-dialog modifier="rowfooter" :visible.sync="editFinancialYear">
			        <span slot="title">Edit Financial Year</span>
			        <v-ons-input type="text" float placeholder="Financial Year Name" v-model="year.name" style="width:100%;font-family:Zawgyi-One">
			        </v-ons-input>
			        <template slot="footer">
			          <div class="alert-dialog-button" @click="editFinancialYear = false">Cancel</div>
			          <div class="alert-dialog-button"  @click="saveFinancialYear" :disabled="isProcessingYear">Save</div>
			        </template>
			      </v-ons-alert-dialog>

  </v-ons-page>
</template>


<script>
	import Auth from '../../../store/auth'
	import { get, del, post, apiDomain } from '../../../helpers/api'
	import { toMulipartedForm } from '../../../helpers/form'
	import MonthShow from '../Month/Show.vue'
  import Yearly from './YearlyChart.vue'
	export default {
    components: {
      Yearly
    },
		data() {
			return {
				animation: 'default',
				// Edit Year
				editFinancialYear: false,
				isProcessingYear: false,
				editYear: { 'name':'' },
        apiUrl: apiDomain,
				authState: Auth.state,
				isRemoving: false,
	      isProcessing: false,
	      addMonth: false,
				months: [],
				monthBalance: [],
				form: {},
				error: {},
				storeURL: apiDomain + `/months`,
				action: 'Create',
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
				}
			}
		},
		created() {
			get(apiDomain + `/years/${this.year.id}`)
				.then((res) => {
					this.months = res.data.months
					this.monthBalance = res.data.monthBalance
					this.form = res.data.form
				})
		},
		computed: {
      OpenBalance() {
          return this.months.reduce(function(carry, month) {
              return carry + parseFloat(month.openBalance)
          }, 0)
      },
      inTotal() {
          return this.months.reduce(function(carry, month) {
            if (month.in_total != null) {
              return carry + parseFloat(month.in_total)
            }else {
              return month.in_total = 0
            }
          }, 0)
      },
      outTotal() {
          return this.months.reduce(function(carry, month) {
            if (month.out_total != null) {
              return carry + parseFloat(month.out_total)
            }else {
              return month.out_total = 0
            }
          }, 0)
      },
      Balance() {
          return (this.OpenBalance + this.inTotal) - parseFloat(this.outTotal)
      }
		},
		methods: {
			saveFinancialYear() {
				this.editYear.name = this.year.name
				post(apiDomain + `/years/${this.year.id}/yearUpdate?_method=PUT`, this.editYear)
						.then((res) => {
								if(res.data.saved) {
										// this.year.name.push("res.data.year.name");
										this.editFinancialYear = false;
								}
								this.isProcessingYear = false
						})
						.catch((err) => {
								if(err.status === 422) {
										this.error = err.data
								}
								this.isProcessingYear = false
						})
			},
			saveBalance() {
					this.year.open_balance = this.OpenBalance
					this.year.balance = this.Balance
					const year = toMulipartedForm(this.year, 'edit')
					post(apiDomain + `/years/${this.year.id}/balance?_method=PUT`, year)
					    .then((res) => {
					        if(res.data.saved) {
											this.year.push(res.data.year)
					        }
					        this.isProcessing = false
					    })
					    .catch((err) => {
					        if(err.status === 422) {
					            this.error = err.data
					        }
					        this.isProcessing = false
					    })
			},
			save() {
				this.form.year_id = this.year.id
				const form = toMulipartedForm(this.form, 'edit')
				post(this.storeURL, form)
				    .then((res) => {
				        if(res.data.saved) {
				            // Flash.setSuccess(res.data.message)
										this.months.push(res.data.month)
										this.addMonth = false
										// location.reload(true)
				            // this.$router.push(`/months/${res.data.id}`)
				        }
				        this.isProcessing = false
				    })
				    .catch((err) => {
				        if(err.status === 422) {
				            this.error = err.data
				        }
				        this.isProcessing = false
				    })
			},
			remove() {
				this.isRemoving = false
				del(apiDomain + `/years/${this.year.id}`)
					.then((res) => {
						if(res.data.deleted) {
							// Flash.setSuccess('You have successfully deleted year!')
							this.$router.push('/years')
						}
					})
			},
			financialMonth(name, data, apiUrl) {
				this.$store.commit('navigator/options', {
					// Sets animations
					animation: name,
					month: data,
					// Resets default options
					callback: () => this.$store.commit('navigator/options', {})
				});

				this.$store.commit('navigator/push', {
					extends: MonthShow,
					data() {
						return {
							animation: name,
							month: data,
							title: data.name,
              reportsource: `months/${data.year_id}/reports/${data.id}`,
              insource: apiUrl + `/months/incomes/${data.id}`,
              outsource: apiUrl + `/months/expenses/${data.id}`,
						}
					}
				});
			}
		}
	}
</script>

<style scoped>
.button-margin {
  margin: 6px 0;
}
</style>
