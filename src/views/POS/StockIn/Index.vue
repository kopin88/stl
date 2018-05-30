<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="title">
      <!-- <template slot="right">
        <v-ons-icon style="color:white; padding-right:10px" icon="md-edit"  @click="editFinancialYear = true"></v-ons-icon>
      </template> -->
    </custom-toolbar>
  <data-viewer :source="source" :thead="thead" :filter="filter" :create="create" :title="title" style="padding-bottom:80px">
      <template slot-scope="props">
          <tr @click="StockInShowPush(animation, props.item)">
              <td>{{ props.item.date | moment("DD - MMM - YYYY") }}</td>
              <td>{{ props.item.invoice_no }}</td>
              <td>{{props.item.supplier.name}}</td>
              <!-- <td>{{props.item.discount}}</td> -->
              <td class="text-right">{{ props.item.total }}</td>
          </tr>
      </template>
  </data-viewer>
  <v-ons-fab position="bottom right" @click="stockInAdd(animation)">
    <v-ons-icon icon="md-plus"></v-ons-icon>
  </v-ons-fab>
</v-ons-page>
</template>


<script>
    import Vue from 'vue'
    import DataViewer from '../../../components/StockDataViewer.vue'
    import StockInShow from './Show.vue'
    import StockInForm from './Form.vue'
    import { apiDomain } from '../../../helpers/api'
    Vue.use(require('vue-moment'));

    export default {
        name: 'StockInIndex',
        data() {
            return {
                title: 'Stock In List',
                // source: apiDomain + '/stockins',
                create: apiDomain + '/stockins/create',
                thead: [
                    {title: '      Date   ', key: 'date', sort: true},
                    {title: 'Invoice', key: 'invoice_no', sort: true},
                    {title: 'Supplier', key: 'supplier.name', sort: false},
                    // {title: 'Discount', key: 'discount', sort: true},
                    {title: 'Total', key: 'total', sort: true}
                ],
                filter: [
                  'invoice_no', 'date', 'total', 'discount',
                  // Customer
                  'supplier.name'
                ]
            }
        },
        components: {
            DataViewer
        },
        methods: {
          StockInShowPush(name, data) {
    				this.$store.commit('navigator/options', {
    					// Sets animations
    					animation: name,
    					stockin: data,
    					// Resets default options
    					callback: () => this.$store.commit('navigator/options', {})
    				});

    				this.$store.commit('navigator/push', {
    					extends: StockInShow,
    					data() {
    						return {
    							animation: name,
    							stockin: data
    						}
    					}
    				});
    			},
          stockInAdd(name) {
    				this.$store.commit('navigator/options', {
    					// Sets animations
    					animation: name,
    					// month: data,
    					// Resets default options
    					callback: () => this.$store.commit('navigator/options', {})
    				});

    				this.$store.commit('navigator/push', {
    					extends: StockInForm,
    					data() {
    						return {
    							animation: name,
    							// month: data,
    							title: "New Invoice",
                  meta: 'create'
    						}
    					}
    				});
    			},
        }
    }
</script>
