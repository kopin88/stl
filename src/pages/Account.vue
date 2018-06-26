<template>
  <v-ons-page>
    <data-viewer  :source="source" :thead="thead" :filter="filter" :create="create" :title="title">
      <template slot-scope="props">
        <v-ons-list>
          <v-ons-list-item :action="onAction" :key="props.item.id" @click="financialYear(animation, props.item)" modifier="chevron" class="list-item-container">
            <div class="left" style="width:100px;">
              <span>{{ props.item.name }}</span>
              <!-- <img class="list-item__thumbnail" :src="imgLink + props.item.image" /> -->
            </div>
            <div class="center">
              <span class="list-item__subtitle">စာရင္းဖြင့္ : {{ props.item.open_balance }}</span>
              <span class="list-item__subtitle">လက္က်န္ : {{ props.item.balance }}</span>
            </div>
            <div class="right">
              <!-- {{props.item.qty_total}} -->
            </div>
          </v-ons-list-item>
        </v-ons-list>

        <!-- <tr @click="financialYear(animation, props.item)" :key="props.item.id">
          <td class="text-center">{{props.item.name}}</td>
          <td class="text-right"><span style="padding-right:40px">{{props.item.open_balance}}</span></td>
          <td class="text-right"><span style="padding-right:40px">{{props.item.balance}}</span></td>
        </tr> -->
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
                    {title: 'Financial Year', key: 'name', sort: true},
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
          onPull(ratio) {
            this.ratio = ratio;
          },
          onAction(done) {
            setTimeout(() => {
              this.items = [...this.items];
              done();
            }, 1500);
          },
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
