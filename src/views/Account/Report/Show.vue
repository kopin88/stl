<template>
	<div>
		<div class="card">
			<div class="card-content">
				<div class="row" style="margin-left:20px">
					<div class="col-md-3 col-xs-12">
						<div class="form__group">
							<label>Financial Yaer</label>
							<span class="from__label">{{month.year.name}}</span>
						</div>
					</div>
					<div class="col-md-3 col-xs-12">
						<div class="form__group">
							<label>Month</label>
              <select class="form__control" v-model="selectMonth">
								<option value="" disabled selected><label>Select ...</label></option>
								<option v-for="month in months" :value="month.id">
									{{month.name}}
								</option>
							</select>
						</div>
					</div>
					<div class="col-md-3 col-xs-12">
						<div class="row">
							<div class="pull-right" style="margin-top:30px">
								<button class="btn btn-default btn-round btn-sm" @click="$router.back()" :disabled="isProcessing">Back</button>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		<div class="card" style="margin-top:0">
  		    <div class="card-header card-header-tabs" data-background-color="green">
  		        <div class="nav-tabs-navigation">
  		            <div class="nav-tabs-wrapper">
										<div class="row">
											<div class="col-xs-4 col-md-2">
												<span class="nav-tabs-title"><strong>{{month.year.name}}/{{month.name}}</strong>:</span>
											</div>
											<div class="col-xs-8">
												<ul class="nav nav-tabs" data-tabs="tabs">
													<li class="active">
														<a href="#IncomeDataView" data-toggle="tab">
															<i class="fa fa-check-square fa-lg" aria-hidden="true"></i>
															<strong>Income</strong>
															<div class="ripple-container"></div>
														</a>
													</li>
													<li class="">
														<a href="#ExpenseDataView" data-toggle="tab">
															<i class="fa fa-check-square fa-lg" aria-hidden="true"></i>
															<strong>Expense</strong>
															<div class="ripple-container"></div>
														</a>
													</li>
													<li class="">
														<a href="#BalanceReportView" data-toggle="tab">
															<i class="fa fa-check-square fa-lg" aria-hidden="true"></i>
															<strong>Report</strong>
															<div class="ripple-container"></div>
														</a>
													</li>
												</ul>
											</div>
										</div>
  		            </div>
  		        </div>
  		    </div>
  		    <div class="card-content">
  		      <div class="tab-content">
  		        <div class="tab-pane in active" id="IncomeDataView">
  		          <div class="row">
  		             <income-data :insource = "`/api/months/incomes/${this.$route.params.month}`" style="margin-top:-20px"></income-data>
  		          </div>
  		        </div>
  		        <div class="tab-pane" id="ExpenseDataView">
  		          <div class="row">
                  <expense-data :outsource = "`/api/months/expenses/${this.$route.params.month}`" style="margin-top:-20px"></expense-data>
  		          </div>
  		        </div>
  		        <div class="tab-pane" id="BalanceReportView">
  		          <div class="row">
									<div class="col-md-12">
										<month-report :reportsource = "`months/reports/${this.$route.params.id}`" style="margin-top:-20px"></month-report>
									</div>
  		          </div>
  		        </div>
  		      </div>
  		    </div>
  		</div>
	</div>
</template>

<script type="text/javascript">
	import { get, del } from '../../../helpers/api'
	import IncomeData from '../Report/IncomeData.vue'
	import ExpenseData from '../Report/ExpenseData.vue'
	import MonthReport from '../Report/Report.vue'
	export default {
    components: {
      IncomeData, ExpenseData, MonthReport
    },
		data() {
			return {
				isProcessing: false,
        years: [],
				months: [],
				month: {
					year: {}
				},
				openBalance: 5000,
				selectYear: this.$route.params.year,
				selectMonth: this.$route.params.month
			}
		},
		created() {
			get(`/api/reports/${this.$route.params.year}/${this.$route.params.month}`)
			.then((res) => {
				this.month = res.data.month
				this.months = res.data.months
				this.openBalance = res.data.openBalance
			})
			get(`/api/reports`)
				.then((res) => {
					this.years = res.data.years
				})
		},
    watch: {
        selectMonth() {
					if (this.selectMonth != this.$route.params.month) {
						this.searchReportMonth()
					}
        }
    },
    methods: {
      searchReportMonth: function() {
        this.$router.push(`/reports/${this.selectYear}/${this.selectMonth}`)
				location.reload(true);
      }
    }
	}
</script>
