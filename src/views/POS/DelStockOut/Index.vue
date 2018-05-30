<template>
<v-ons-page>
  <page-toolbar :title="title"></page-toolbar>
  <data-viewer :source="source" :thead="thead" :filter="filter" :create="create" :title="title" style="margin-top:56px">
      <template slot-scope="props">
          <tr @click="$router.push('/stockouts/' + props.item.id)">
              <td>{{ props.item.date | moment("DD - MMM - YYYY") }}</td>
              <td>{{ props.item.invoice_no }}</td>
              <!-- <td>{{props.item.customer.name}}</td>
              <td>{{props.item.discount}}</td> -->
              <td class="text-right">{{ props.item.total }}</td>
          </tr>
      </template>
  </data-viewer>
  <v-ons-fab position="bottom right" @click="$router.push('/stockouts/create')">
    <v-ons-icon icon="md-plus"></v-ons-icon>
  </v-ons-fab>
</v-ons-page>
</template>


<script>
    import Vue from 'vue'
    import DataViewer from '../../../components/StockDataViewer.vue'
    import PageToolbar from '../../Layout/Toolbar.vue'
    import { apiDomain } from '../../../helpers/api'
    Vue.use(require('vue-moment'));

    export default {
        name: 'StockOutIndex',
        data() {
            return {
                title: 'Stock Out List',
                source: apiDomain + '/stockouts',
                create: apiDomain + '/stockouts/create',
                thead: [
                    {title: '    Date   ', key: 'date', sort: true},
                    {title: 'Invoice', key: 'invoice_no', sort: true},
                    // {title: 'Supplier', key: 'customer.name', sort: false},
                    // {title: 'Discount', key: 'discount', sort: true},
                    {title: 'Total', key: 'total', sort: true}
                ],
                filter: [
                  'invoice_no', 'date', 'total', 'discount',
                  // Customer
                  'customer.name'
                ]
            }
        },
        components: {
            DataViewer,
            PageToolbar
        }
    }
</script>


<style scoped>
.img {
  height: 80px;
  width: 80px;
  border-radius: 10%;
}

/* .fab {
  background-color: green;
  color: white;
} */
</style>
