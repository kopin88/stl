<template>
  <v-ons-page>
    <data-viewer  :source="source" :thead="thead" :filter="filter" :create="create" :title="title">
      <template slot-scope="props">
        <tr @click="financialYear(animation, props.item)" :key="props.item.id">
          <td class="text-center">{{props.item.name}}</td>
          <td class="text-right"><span style="padding-right:40px">{{props.item.open_balance}}</span></td>
          <td class="text-right"><span style="padding-right:40px">{{props.item.balance}}</span></td>
        </tr>
      </template>
    </data-viewer>

      <v-ons-fab position="bottom right" @click="createFinancialYear(animation)">
        <v-ons-icon icon="md-plus"></v-ons-icon>
      </v-ons-fab>


  </v-ons-page>
</template>
<script>
    import DataViewer from '../components/FinancialDataViewer.vue'
    import { get, post, apiDomain } from '../helpers/api'
  	import YearShow from '../views/Account/Year/Show.vue'
  	import YearForm from '../views/Account/Year/Form.vue'
    export default {
        data() {
            return {
                animation: 'default',
                title: 'Financial Year List',
                create: '/years/create',
                source: apiDomain + '/years',
                thead: [
                    {title: 'FinancialYear', key: 'name', sort: true},
                    {title: 'Opening', key: 'open_balance', sort: true},
                    {title: 'Balance', key: 'balance', sort: true},
                    // {title: 'Remark', key: 'remark', sort: true},
                ],
                filter: [
                  'name', 'open_balance', 'balance'
                ],
            }
        },
    		methods: {
          createFinancialYear(name) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              // year: data,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: YearForm,
              data() {
                return {
                  animation: name,
                  // year: data,
                  title: "ဘ႑ာေရးႏွစ္"
                }
              }
            });
          },
          financialYear(name, data) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              year: data,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: YearShow,
              data() {
                return {
                  animation: name,
                  year: data,
                  title: data.name + "  (ဘ႑ာေရးႏွစ္)"
                }
              }
            });
          }
    		},
        components: { DataViewer }
    }
</script>
