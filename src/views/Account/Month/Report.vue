<template>
	<div class="table-responsive">
		<table class="table table-bordered" width="100%" style="width: 500px; margin-bottom: 80px">
		<tbody>
			<tr>
				<th style="width:10%"></th>
				<th style="width:50%">ေငြစာရင္းေခါင္းစဥ္</th>
				<th style="width:20%">Amount</th>
				<th style="width:20%">မွတ္ခ်က္</th>
			</tr>
			<tr>
				<td colspan="4" class="theme__color"></td>
			</tr>
			<tr>
				<th></th>
				<th>Opening Balance</th>
				<th class="table-number text-warning" v-if="month.open_balance < 100">{{ month.open_balance }}</th>
				<th class="table-number" v-else>{{ month.open_balance }}</th>
				<th></th>
			</tr>
			<tr>
				<td colspan="4" class="theme__color"></td>
			</tr>
			<tr>
				<th></th>
				<th>Income</th>
				<th></th>
				<th></th>
			</tr>
			<tr v-for="inreport in inreports">
				<td></td>
				<td>{{inreport.in_head.name}}</td>
				<td class="table-number">{{inreport.total}}</td>
				<td></td>
			</tr>
			<tr>
				<th></th>
				<th>Total incomes</th>
				<th class="table-number">{{month.in_total}}</th>
				<th class="text-center">
					<v-ons-icon class="text-warning" icon="md-alert-circle" v-if="month.in_total != inTotal" @click="saveInTotal" :disabled="isProcessing"></v-ons-icon>
					<v-ons-icon class="text__color" icon="md-check-circle" v-else></v-ons-icon>
				</th>
			</tr>
			<tr><td colspan="4" class="theme__color"></td></tr>
			<tr>
				<th></th>
				<th>Expense</th>
				<th></th>
				<th></th>
			</tr>
			<tr v-for="outreport in outreports">
				<td></td>
				<td>{{outreport.out_head.name}}</td>
				<td class="table-number">{{outreport.total}}</td>
				<th></th>
			</tr>
			<tr>
				<th></th>
				<th>Total expenses</th>
				<th  class="table-number">{{month.out_total}}</th>
				<th class="text-center">
					<v-ons-icon class="text-warning" icon="md-alert-circle"  v-if="month.out_total != outTotal" @click="saveOutTotal" :disabled="isProcessing"></v-ons-icon>
					<v-ons-icon class="text__color" icon="md-check-circle" v-else></v-ons-icon>
				</th>
			</tr>
			<tr><td colspan="4" class="theme__color"></td></tr>
			<tr>
				<th></th>
				<th>Balance</th>
				<th class="table-number text-danger" v-if="Balance < 0">{{Balance}}</th>
				<th class="table-number" v-else>{{Balance}}</th>
				<th></th>
			</tr>
			<tr><td colspan="4" class="theme__color"></td></tr>
			<tr>
				<th></th>
				<th>Total Balance</th>
				<th class="table-number text-danger" v-if="balanceTotal < 0">{{balanceTotal}}</th>
				<th class="table-number" v-else>{{month.balance}}</th>
				<th class="text-center">
					<v-ons-icon class="text-warning" icon="md-alert-circle"  v-if="month.balance != balanceTotal" @click="saveBalance" :disabled="isProcessing"></v-ons-icon>
					<v-ons-icon class="text__color" icon="md-check-circle" v-else></v-ons-icon>
				</th>
			</tr>
		</tbody>
	</table>
	</div>
</template>

<script type="text/javascript">
	import { get, del, post, apiDomain } from '../../../helpers/api'
	import { toMulipartedForm } from '../../../helpers/form'
	export default {
      props: ['reportsource'],
		data() {
			return {
				isProcessing: false,
        inreports: [],
        outreports:[],
        inheads: [],
        outheads: [],
				// openBalance:'',
				month: {
					year:{}
				},
				// storeInURL: apiDomain + `/months/${this.$route.params.id}/incomes?_method=PUT`,
				// storeOutURL: apiDomain + `/months/${this.$route.params.id}/expenses?_method=PUT`,
				// storeURL: apiDomain + `/months/${this.$route.params.id}/balance?_method=PUT`,
			}
		},
		created() {
			get(apiDomain + `/${this.reportsource}`)
				.then((res) => {
					this.inreports = res.data.inreports
					this.outreports = res.data.outreports
					this.inheads = res.data.inheads
					this.outheads = res.data.outheads
					this.month = res.data.month
					// this.openBalance = res.data.openBalance
				})
		},
		computed: {
			inTotal() {
					return this.inreports.reduce(function(carry, inreport) {
							return carry + parseFloat(inreport.total)
					}, 0)
			},
			outTotal() {
					return this.outreports.reduce(function(carry, outreport) {
							return carry + parseFloat(outreport.total)
					}, 0)
			},
			Balance() {
					return this.inTotal - parseFloat(this.outTotal)
			},
			balanceTotal() {
					return this.inTotal + parseFloat(this.month.open_balance) - parseFloat(this.outTotal)
					// return this.inTotal + this.open_balance - this.outTotal
			}
		},
		methods: {
			saveInTotal() {
					this.month.in_total = this.inTotal
					const month = toMulipartedForm(this.month, 'edit')
					post(apiDomain + `/months/${this.month.id}/incomes?_method=PUT`, month)
					    .then((res) => {
					        if(res.data.saved) {
											this.month.push(res.data.month)
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
			saveOutTotal() {
					this.month.out_total = this.outTotal
					const month = toMulipartedForm(this.month, 'edit')
					post(apiDomain + `/months/${this.month.id}/expenses?_method=PUT`, month)
					    .then((res) => {
					        if(res.data.saved) {
											this.month.push(res.data.month)
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
			saveBalance() {
					this.month.balance = this.balanceTotal
					const month = toMulipartedForm(this.month, 'edit')
					post(apiDomain + `/months/${this.month.id}/balance?_method=PUT`, month)
					    .then((res) => {
					        if(res.data.saved) {
											this.month.push(res.data.month)
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
		}
	}
</script>
