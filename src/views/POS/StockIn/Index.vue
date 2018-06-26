<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="title">
      <template slot="right">
        <v-ons-icon style="color:white" icon="md-search" @click="searchStockIn(animation)"></v-ons-icon>
      </template>
    </custom-toolbar>
  <data-viewer :source="source" :thead="thead" :filter="filter" :create="create" :title="title" style="padding-bottom:80px">
      <template slot-scope="props">
        <v-ons-list>
          <v-ons-list-item :action="onAction" :key="props.item.id" @click="StockInShowPush(animation, props.item)" modifier="chevron" class="list-item-container">
            <div class="left">
              {{ props.item.date | moment("DD-MMM-YYYY") }}
            </div>
            <div class="center" style="margin-left:10px">
              <span class="list-item__title">{{ props.item.invoice_no }}</span>
              <span class="list-item__subtitle">Supplier : {{props.item.supplier.name}}</span>
              <!-- <span class="list-item__subtitle">price : {{ props.item.sale_price }}</span> -->
            </div>
            <div class="right">
              {{ props.item.total }}
            </div>
          </v-ons-list-item>
        </v-ons-list>
      </template>
  </data-viewer>
  <v-ons-fab position="bottom right" @click="stockInAdd(animation)">
    <v-ons-icon icon="md-plus"></v-ons-icon>
  </v-ons-fab>
</v-ons-page>
</template>


<script>
    import Vue from 'vue'
    import DataViewer from '../../../components/StockInDataViewer.vue'
    import StockInShow from './Show.vue'
    import StockInSearch from './Search.vue'
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
                    {title: 'Voucher No', key: 'invoice_no', sort: true},
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
          onPull(ratio) {
            this.ratio = ratio;
          },
          onAction(done) {
            setTimeout(() => {
              this.stockins = [...this.stockins];
              done();
            }, 1500);
          },
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
          searchStockIn(name) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: StockInSearch,
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
