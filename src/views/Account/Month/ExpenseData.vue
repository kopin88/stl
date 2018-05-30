<template>
  <div style="margin-bottom:80px">
    <income-expense-data-viewer :source="source" :thead="thead" :filter="filter">
      <template slot-scope="props">
        <tr>
        <tr @click="expenseShow(animation, props.item, apiUrl, month)">
          <td>{{ props.item.date | moment("DD - MMM - YYYY") }}</td>
          <td>{{ props.item.name }}</td>
          <!-- <td>{{ props.item.description }}</td>
          <td>{{ props.item.out_head.name }}</td> -->
          <td class="text-right">{{ props.item.amount }}</td>
        </tr>
      </template>
    </income-expense-data-viewer>
  </div>
</template>
<script>
    import Vue from 'vue'
    import Auth from '../../../store/auth'
    import IncomeExpenseDataViewer from '../../../components/IncomeExpenseDataViewer.vue'
  	import { del, apiDomain } from '../../../helpers/api'
  	import ExpenseShow from '../Expense/Show.vue'
    Vue.use(require('vue-moment'));
    export default {
        props: ['outsource', 'month'],
        name: 'ExpenseData',
        data() {
            return {
                animation: 'default',
                apiUrl: apiDomain,
                source: this.outsource,
                authState: Auth.state,
                isRemoving: false,
        	      isProcessing: false,
                thead: [
                    {title: 'Date', key: 'date', sort: true},
                    {title: 'Name', key: 'name', sort: true},
                    // {title: 'Description', key: 'description', sort: true},
                    // {title: 'Account Head', key: 'out_head_id', sort: true},
                    {title: 'Amount', key: 'amount', sort: true},
                    // {title: 'Action', key: 'action', sort: false},
                ],
                filter: [
                  'date', 'name', 'amount'
                ]
            }
        },
        components: {
            IncomeExpenseDataViewer
        },
        methods: {
          expenseShow(name, data, apiUrl, month) {
    				this.$store.commit('navigator/options', {
    					// Sets animations
    					animation: name,
    					expense: data,
              month: month,
    					// Resets default options
    					callback: () => this.$store.commit('navigator/options', {})
    				});

    				this.$store.commit('navigator/push', {
    					extends: ExpenseShow,
    					data() {
    						return {
    							animation: name,
    							expense: data,
                  month: month,
    							title: data.name,
                  // reportsource: `months/${data.year_id}/reports/${data.id}`,
                  // insource: apiUrl + `/months/incomes/${data.id}`,
                  // outsource: apiUrl + `/months/expenses/${data.id}`,
    						}
    					}
    				});
    			}
        }
    }
</script>
