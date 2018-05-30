<template>
  <div style="margin-bottom:80px">
    <income-expense-data-viewer :source="source" :thead="thead" :filter="filter">
      <template slot-scope="props">
        <tr>
        <tr @click="incomeShow(animation, props.item, apiUrl, month)">
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
  	import IncomeShow from '../Income/Show.vue'
    Vue.use(require('vue-moment'));
    export default {
        props: ['insource', 'month'],
        name: 'IncomeData',
        data() {
            return {
                animation: 'default',
                apiUrl: apiDomain,
                source: this.insource,
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
          incomeShow(name, data, apiUrl, month) {
    				this.$store.commit('navigator/options', {
    					// Sets animations
    					animation: name,
    					income: data,
              month: month,
    					// Resets default options
    					callback: () => this.$store.commit('navigator/options', {})
    				});

    				this.$store.commit('navigator/push', {
    					extends: IncomeShow,
    					data() {
    						return {
    							animation: name,
    							income: data,
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
