<template>
	<div>
		<div class="card">
			<div class="card-content">
				<div class="row" style="margin-left:20px">
					<div class="col-md-3 col-xs-12">
						<div class="form__group">
							<label>Financial Yaer</label>
							<select class="form__control" v-model="selectYear">
								<option value="" disabled selected><label>Search ...</label></option>
								<option v-for="year in years" :value="year.id">
									{{year.name}}
								</option>
							</select>
						</div>
					</div>
					<div class="col-md-3 col-xs-12" style="margin-buttom:5px">
						<div class="form__group">
							<label>Month</label>
							<select class="form__control" v-model="selectMonth">
								<option value="" disabled selected><label>Select ...</label></option>
								<option value="" disabled><label> Please select financial year ... </label></option>
								<option v-for="month in months" :value="month.id">
									{{month.name}}
								</option>
							</select>
						</div>
					</div>
				</div>
			</div>
		</div>
    <div class="card" style="margin-top:0" v-show="selectYear.length != 0">
  		    <div class="card-header card-header-tabs" data-background-color="green">
            <span class="card-title"><strong>Financial Year</strong></span>
  		    </div>
  		    <div class="card-content">
            <div class="row" style="margin-left:20px">
              <div class="col-md-12 col-xs-12">
                <div class="table-responsive">
                  <table class="table table-bordered">
                    <thead>
                      <tr>
                        <th>Month Name</th>
                        <th>Title</th>
                        <th>Opening Balance</th>
                        <th>Income</th>
                        <th>Expense</th>
                        <th>Total</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="month in months" :key="month.id" @click="$router.push('/reports/' + month.id)">
                        <td>{{ month.name }}</td>
                        <td>{{ month.title }}</td>
                        <td>{{ month.open_balance }}</td>
                        <td>{{ month.in_total }}</td>
                        <td>{{ month.out_total }}</td>
                        <td>{{ month.balance }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
  		    </div>
  		</div>
	</div>
</template>

<script type="text/javascript">
	import { get, del } from '../../../helpers/api'
	export default {
		data() {
			return {
        years: [],
        selectYear: '',
        months: [],
        selectMonth: '',
			}
		},
		created() {
			get(`/api/reports`)
				.then((res) => {
					this.years = res.data.years
				})
		},
    watch: {
        selectYear() {
          this.selectMonth = ''
          if (this.selectMonth.length == 0 ) {
            this.searchReportYear()
          }
        },
        selectMonth() {
          this.searchReportMonth()
        }
    },
    methods: {
      searchReportYear: function() {
        get(`/api/reports/${this.selectYear}`)
  				.then((res) => {
  					this.months = res.data.months
  				})
      },
      searchReportMonth: function() {
        this.$router.push(`/reports/${this.selectYear}/${this.selectMonth}`)
      }
    }
	}
</script>
