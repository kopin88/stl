<template>
    <income-expense-data-viewer :source="source" :thead="thead" :filter="filter">
        <template scope="props">
          <tr>
            <td>{{ props.item.date | moment("DD - MMM - YYYY") }}</td>
            <td>{{ props.item.name }}</td>
            <td>{{ props.item.description }}</td>
            <td>{{ props.item.out_head.name }}</td>
            <td class="text-right"><span style="padding-right:40px">{{ props.item.amount }}</span></td>
            <!-- <td class="text-center">
              <button v-if="authState.api_token" @click="$router.push('/expenses/' + props.item.id )" class="btn btn-success btn-round btn-circle btn-xs" style="padding:3px;"><i class="material-icons">library_books</i></button>
              <button v-if="authState.api_token" type="button" @click="$router.push('/expenses/' + props.item.id + '/edit')" rel="tooltip" title="Edit" class="btn btn-success btn-round btn-circle btn-xs" style="padding:3px;">
                <i class="material-icons">edit</i>
              </button>
              <button v-if="authState.api_token" @click="$router.push('/expenses/' + props.item.id )" class="btn btn-danger btn-round btn-circle btn-xs" style="padding:3px;"><i class="material-icons">delete</i></button>
            </td> -->
          </tr>
        </template>
    </income-expense-data-viewer>
</template>
<script>
    import Vue from 'vue'
    import Auth from '../../../store/auth'
    import IncomeExpenseDataViewer from '../../../components/IncomeExpenseDataViewer.vue'
  	Vue.use(require('../../../helpers/vue-moment'))
    export default {
        props: ['outsource'],
        name: 'ExpenseData',
        data() {
            return {
                source: this.outsource,
                authState: Auth.state,
                isRemoving: false,
        	      isProcessing: false,
                thead: [
                    {title: 'Date', key: 'date', sort: true},
                    {title: 'Name', key: 'name', sort: true},
                    {title: 'Description', key: 'description', sort: true},
                    {title: 'Account Head', key: 'out_head_id', sort: true},
                    {title: 'Amount', key: 'amount', sort: true},
                    // {title: 'Action', key: 'action', sort: false},
                ],
                filter: [
                  'date','out_head.name', 'name', 'description', 'amount'
                ]
            }
        },
        components: {
            IncomeExpenseDataViewer
        }
    }
</script>
