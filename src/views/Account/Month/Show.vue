<template id="main">
  <v-ons-page>
    <custom-toolbar backLabel="Anim" :title="title">
      <template slot="right">
        <v-ons-icon style="color:white; padding-right:10px" icon="md-edit" @click="editFinancialMonth(animation, month)"></v-ons-icon>
      </template>
    </custom-toolbar>
			<v-ons-card>
				<v-ons-row width="100%">
					<v-ons-col width="30%"><v-ons-text>ေငြစာရင္း :  </v-ons-text>
					</v-ons-col>
					<v-ons-col width="60%"><v-ons-text>
						{{ month.title }}
						</v-ons-text>
					</v-ons-col>
					<v-ons-col width="10%">
            <!-- <v-ons-icon style="color:green;" icon="md-edit" @click="editFinancialMonth(animation, month)"></v-ons-icon> -->
					</v-ons-col>
				</v-ons-row>
				<v-ons-row>
					<v-ons-col><v-ons-text>စာရင္းဖြင့္ : </v-ons-text>
					</v-ons-col>
					<v-ons-col><v-ons-text class="pull-right">{{ month.open_balance }}</v-ons-text>
					</v-ons-col>
					<v-ons-col>
					</v-ons-col>
				</v-ons-row>
				<v-ons-row>
					<v-ons-col><v-ons-text>လက္က်န္ : </v-ons-text>
					</v-ons-col>
					<v-ons-col><v-ons-text class="pull-right">{{ month.balance }}</v-ons-text>
					</v-ons-col>
					<v-ons-col>
					</v-ons-col>
				</v-ons-row>


				<div class="center" style="margin-top:50px">
	        <v-ons-segment :index.sync="segmentIndex" style="width: 100%">
	          <button @click="ReportDataView">Report</button>
	          <button @click="IncomeDataView">Income</button>
	          <button @click="ExpenseDataView">Expense</button>
	        </v-ons-segment>
	      </div>
				<v-ons-text v-show="ReportShow">
					<report-data :reportsource = "reportsource" style="margin-top:20px"></report-data>
				</v-ons-text>
				<v-ons-text v-show="IncomeShow">
					<income-data :month="month" :insource = "insource" style="margin-top:20px"></income-data>
				</v-ons-text>
				<v-ons-text v-show="ExpenseShow">
					<expense-data :month="month" :outsource = "outsource" style="margin-top:20px"></expense-data>
				</v-ons-text>
			</v-ons-card>


      <v-ons-fab v-show="IncomeShow" position="bottom right" @click="incomeAdd(animation, month, apiUrl)">
        <v-ons-icon icon="md-plus"></v-ons-icon>
      </v-ons-fab>

      <v-ons-fab v-show="ExpenseShow" position="bottom right" @click="expenseAdd(animation, month, apiUrl)">
        <v-ons-icon icon="md-plus"></v-ons-icon>
      </v-ons-fab>

	</v-ons-page>
</template>

<script type="text/javascript">
	import Auth from '../../../store/auth'
	// import Flash from '../../../helpers/flash'
	import { get, del, post, apiDomain } from '../../../helpers/api'
	import { toMulipartedForm } from '../../../helpers/form'
  import MonthEditForm from './MonthEditForm.vue'
	import IncomeData from './IncomeData.vue'
	import ExpenseData from './ExpenseData.vue'
	import ReportData from './Report.vue'
	import IncomeForm from '../Income/Form.vue'
	import ExpenseForm from '../Expense/Form.vue'
  import ImageUpload from '../../../components/ImageUpload.vue'

	export default {
		directives: { focus: focus },
		components: { MonthEditForm, IncomeData, ExpenseData, ReportData, ImageUpload },
		data() {
			return {
        apiUrl: apiDomain,
        animation: 'default',
				ReportShow: true,
				IncomeShow: false,
				ExpenseShow: false,
				segmentIndex:0,
				authState: Auth.state,
				isRemoving: false,
	      isProcessing: false,
				// month: { year: '' },
				openBalance: {}
			}
		},
		created() {
			get(apiDomain + `/${this.month.year_id}/months/${this.month.id}`)
				.then((res) => {
					this.openBalance = res.data.openBalance
				})
		},
		methods: {
      editFinancialMonth(name, data) {
				this.$store.commit('navigator/options', {
					// Sets animations
					animation: name,
					form: data,
					// Resets default options
					callback: () => this.$store.commit('navigator/options', {})
				});

				this.$store.commit('navigator/push', {
					extends: MonthEditForm,
					data() {
						return {
							animation: name,
							form: data,
							title: "Month Edit"
						}
					}
				});
			},
      incomeAdd(name, data, apiUrl) {
				this.$store.commit('navigator/options', {
					// Sets animations
					animation: name,
					month: data,
					// Resets default options
					callback: () => this.$store.commit('navigator/options', {})
				});

				this.$store.commit('navigator/push', {
					extends: IncomeForm,
					data() {
						return {
							animation: name,
							month: data,
							title: "New Income",
              meta: 'create'
						}
					}
				});
			},
      expenseAdd(name, data, apiUrl) {
				this.$store.commit('navigator/options', {
					// Sets animations
					animation: name,
					month: data,
					// Resets default options
					callback: () => this.$store.commit('navigator/options', {})
				});

				this.$store.commit('navigator/push', {
					extends: ExpenseForm,
					data() {
						return {
							animation: name,
							month: data,
							title: "New Expense",
              meta: 'create'
						}
					}
				});
			},
      ReportDataView() {
        this.ReportShow = true
        this.IncomeShow = false
        this.ExpenseShow = false
      },
      IncomeDataView() {
        this.ReportShow = false
        this.IncomeShow = true
        this.ExpenseShow = false
      },
      ExpenseDataView() {
        this.ReportShow = false
        this.IncomeShow = false
        this.ExpenseShow = true
      },
			remove() {
				this.isRemoving = false
				del(apiDomain + `/months/${this.month.id}`)
					.then((res) => {
						if(res.data.deleted) {
							// Flash.setSuccess('You have successfully deleted month!')
							// this.$router.push('/months')
						}
					})
			}
		}
	}
</script>
