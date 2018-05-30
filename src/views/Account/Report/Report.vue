<template>
	<div>
		<div class="col-md-8">
			<table class="table table-bordered">
				<tbody>
					<tr>
						<th></th>
						<th>Cash Title</th>
						<th>Amount</th>
						<th>Remark</th>
					</tr>
					<tr>
						<td colspan="4" class="bg-success"></td>
					</tr>
					<tr>
						<th></th>
						<th>Opening Balance</th>
						<th class="table-number">{{ month.openBalance }}</th>
						<th></th>
					</tr>
					<tr>
						<td colspan="4" class="bg-success"></td>
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
						</th>
					</tr>
					<tr><td colspan="4" class="bg-success"></td></tr>
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
						</th>
					</tr>
					<tr><td colspan="4" class="bg-success"></td></tr>
					<tr>
						<th></th>
						<th>Balance</th>
						<th class="table-number" >{{Balance}}</th>
						<th></th>
					</tr>
					<tr><td colspan="4" class="bg-success"></td></tr>
					<tr>
						<th></th>
						<th>Total Balance</th>
						<th class="table-number text-danger" v-if="balanceTotal < 0">{{balanceTotal}}</th>
						<th class="table-number">{{month.balance}}</th>
						<th class="text-center">
						</th>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</template>

<script type="text/javascript">
	import { get, del, post } from '../../../helpers/api'
	import { toMulipartedForm } from '../../../helpers/form'
	export default {
      props: ['reportsource'],
		data() {
			return {
				isProcessing: false,
        inreports: [],
        outreports:[],
        inheads: [],
        month: [],
				month: {
					year:{}
				},
				storeInURL: `/api/months/${this.$route.params.id}/incomes?_method=PUT`,
				storeOutURL: `/api/months/${this.$route.params.id}/expenses?_method=PUT`,
				storeURL: `/api/months/${this.$route.params.id}/balance?_method=PUT`,
			}
		},
		created() {
			get(`/api/${this.reportsource}`)
				.then((res) => {
					this.inreports = res.data.inreports
					this.outreports = res.data.outreports
					this.inheads = res.data.inheads
					this.outheads = res.data.outheads
					this.month = res.data.month
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
					return (this.inTotal + this.month.openBalance) - parseFloat(this.outTotal)
			}
		}
	}
</script>
