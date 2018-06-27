<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="title">
      <template slot="right">
        <v-ons-icon style="color:white" icon="md-search" @click="searchStockOut(animation)"></v-ons-icon>
      </template>
    </custom-toolbar>
  <data-viewer :source="source" :thead="thead" :filter="filter" :create="create" :title="title" style="padding-bottom:80px">
      <template slot-scope="props">
          <tr @click="StockOutShowPush(animation, props.item)">
              <td>{{ props.item.date | moment("DD - MMM - YYYY") }}</td>
              <td>{{ props.item.invoice_no }}</td>
              <td>{{props.item.customer.name}}</td>
              <!-- <td>{{props.item.discount}}</td> -->
              <td class="text-right">{{ props.item.total }}</td>
          </tr>
      </template>
  </data-viewer>
  <v-ons-fab position="bottom right" @click="stockOutAdd(animation)">
    <v-ons-icon icon="md-plus"></v-ons-icon>
  </v-ons-fab>
</v-ons-page>
</template>


<script>
    import Vue from 'vue'
    import DataViewer from '../../../components/StockDataViewer.vue'
    import StockOutShow from './Show.vue'
    import StockOutSearch from './Search.vue'
    import StockOutForm from './Form.vue'
    import { apiDomain } from '../../../helpers/api'
    Vue.use(require('vue-moment'));

    export default {
        name: 'StockOutIndex',
        data() {
            return {
                title: 'Stock Out List',
                // source: apiDomain + '/stockouts',
                create: apiDomain + '/stockouts/create',
                thead: [
                    {title: '      Date   ', key: 'date', sort: true},
                    {title: 'Invoice', key: 'invoice_no', sort: true},
                    {title: 'Customer', key: 'customer.name', sort: false},
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
            DataViewer
        },
        methods: {
          StockOutShowPush(name, data) {
    				this.$store.commit('navigator/options', {
    					// Sets animations
    					animation: name,
    					stockout: data,
    					// Resets default options
    					callback: () => this.$store.commit('navigator/options', {})
    				});

    				this.$store.commit('navigator/push', {
    					extends: StockOutShow,
    					data() {
    						return {
    							animation: name,
    							stockout: data
    						}
    					}
    				});
    			},
          stockOutAdd(name) {
    				this.$store.commit('navigator/options', {
    					// Sets animations
    					animation: name,
    					// month: data,
    					// Resets default options
    					callback: () => this.$store.commit('navigator/options', {})
    				});

    				this.$store.commit('navigator/push', {
    					extends: StockOutForm,
    					data() {
    						return {
    							animation: name,
    							// month: data,
    							title: "Stock Out (New)",
                  meta: 'create'
    						}
    					}
    				});
    			},
          searchStockOut(name) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: StockOutSearch,
              data() {
                return {
                  animation: 'none',
                  title: "",
                }
              }
            });
          }
        }
    }
</script>
