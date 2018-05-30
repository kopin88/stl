<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="'Stock In'">
      <template slot="right">
        <v-ons-icon style="color:white; padding-right:10px" icon="md-edit" @click="StockOutEdit(animation, stockout)"></v-ons-icon>
        <!-- <v-ons-icon style="color:white" icon="md-delete"  @click="remove" :disabled="isRemoving"></v-ons-icon> -->
      </template>
    </custom-toolbar>
  <v-ons-card style="padding-bottom:80px">
    <v-ons-row>
      <v-ons-col><v-ons-text>Invoice No . </v-ons-text>
      </v-ons-col>
      <v-ons-col><v-ons-text>{{ stockout.invoice_no }}</v-ons-text>
      </v-ons-col>
    </v-ons-row>
    <v-ons-row>
      <v-ons-col><v-ons-text>Date . </v-ons-text>
      </v-ons-col>
      <v-ons-col><v-ons-text>{{ stockout.date }}</v-ons-text>
      </v-ons-col>
    </v-ons-row>
    <v-ons-row>
      <v-ons-col><v-ons-text>Supplier . </v-ons-text>
      </v-ons-col>
      <v-ons-col><v-ons-text>{{ stockout.customer.name }}</v-ons-text>
      </v-ons-col>
    </v-ons-row>
      <v-ons-row style="margin-top:10px">
        <div class="table-responsive">
            <table class="table table-bordered table-responsive">
              <thead>
                <tr>
                  <th class="text-right">#</th>
                  <th class="text-center">SelectedItemName</th>
                  <th class="text-center">UnitPrice</th>
                  <th class="text-center">Qty</th>
                  <th class="text-center">Total</th>
                  <th class="text-center"></th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(itemout, index) in stockoutData.itemouts">
                  <td><v-ons-text>{{ index + 1 }}</v-ons-text></td>
                  <td><v-ons-text>{{ itemout.name }}</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ itemout.unit_price }}</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ itemout.qty }}</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ itemout.unit_price * itemout.qty }}</v-ons-text></td>
                  <td></td>
                </tr>
              </tbody>
              <tfoot>
                <tr>
                  <td colspan="3"></td>
                  <td class="text-right"><v-ons-text>Sub Total</v-ons-text></td>
                  <td class="text-right">
                    <v-ons-text>{{ stockout.sub_total }}</v-ons-text>
                  </td>
                  <td></td>
                </tr>
                <tr>
                  <td colspan="3">
                  </td>
                  <td class="text-right"><v-ons-text>Remaining</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ stockout.rem_balance }}</v-ons-text></td>
                  </td>
                  <td>
                  </td>
                </tr>
                <tr>
                  <td colspan="3">
                  </td>
                  <td class="text-right"><v-ons-text>Discount</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ stockout.discount }}</v-ons-text></td>
                  <td></td>
                </tr>
                <tr>
                  <td colspan="3">

                  </td>
                  <td class="text-right"><v-ons-text>Total</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ stockout.total }}</v-ons-text></td>
                  <td></td>
                </tr>
              </tfoot>
            </table>
          </div>
    </v-ons-row>
  </v-ons-card>
  <!-- <img :src="imgLink + stockout.image" style="width: 125px"> -->
</v-ons-page>
</template>

<script type="text/javascript">
import Auth from '../../../store/auth'
// import Flash from '../../../helpers/flash'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import StockOutForm from './Form.vue'
// import ItemIn from './ItemIn.vue'
// import ItemOut from './ItemOut.vue'
export default {
  components: {
    // ItemIn, ItemOut,
    // PageToolbar
  },
  data() {
    return {
      // title: "OK Show Page",
      imgLink: imgUrl + 'stockouts/',
      authState: Auth.state,
      animation: 'default',
      isRemoving: false,
      isProcessing: false,
      stockoutData: {
        customer: {},
        itemouts: []
      }
    }
  },
  created() {
    get( apiDomain + `/stockouts/${this.stockout.id}`)
      .then((res) => {
        this.stockoutData = res.data.stockout
      })
  },
  methods: {
    StockOutEdit(name, data) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        stockout: data,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: StockOutForm,
        data() {
          return {
            animation: name,
            stockout: data,
            title: "Stock Out (Edit)",
            meta: 'edit'
          }
        }
      });
    },
    remove() {
      this.isRemoving = false
      del( apiDomain + `/stockouts/${this.stockout.id}`)
        .then((res) => {
          if (res.data.deleted) {
            // Flash.setSuccess('You have successfully deleted item!')
            this.$router.push('/stockouts')
          }
        })
    }
  }
}
</script>

<style scoped>

</style>
