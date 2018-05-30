<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="'Stock In'">
      <!-- <template slot="right">
        <v-ons-icon style="color:white; padding-right:10px" icon="md-edit"  @click="editFinancialYear = true"></v-ons-icon>
      </template> -->
    </custom-toolbar>
  <v-ons-card style="padding-bottom:80px">
    <v-ons-row>
      <v-ons-col><v-ons-text>Invoice No . </v-ons-text>
      </v-ons-col>
      <v-ons-col><v-ons-text>{{ stockin.invoice_no }}</v-ons-text>
      </v-ons-col>
    </v-ons-row>
    <v-ons-row>
      <v-ons-col><v-ons-text>Date . </v-ons-text>
      </v-ons-col>
      <v-ons-col><v-ons-text>{{ stockin.date }}</v-ons-text>
      </v-ons-col>
    </v-ons-row>
    <v-ons-row>
      <v-ons-col><v-ons-text>Supplier . </v-ons-text>
      </v-ons-col>
      <v-ons-col><v-ons-text>{{ stockin.supplier.name }}</v-ons-text>
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
                <tr v-for="(itemin, index) in stockinData.itemins">
                  <td><v-ons-text>{{ index + 1 }}</v-ons-text></td>
                  <td><v-ons-text>{{ itemin.name }}</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ itemin.unit_price }}</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ itemin.qty }}</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ itemin.unit_price * itemin.qty }}</v-ons-text></td>
                  <td></td>
                </tr>
              </tbody>
              <tfoot>
                <tr>
                  <td colspan="3"></td>
                  <td class="text-right"><v-ons-text>Sub Total</v-ons-text></td>
                  <td class="text-right">
                    <v-ons-text>{{ stockin.sub_total }}</v-ons-text>
                  </td>
                  <td></td>
                </tr>
                <tr>
                  <td colspan="3">
                  </td>
                  <td class="text-right"><v-ons-text>Remaining</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ stockin.rem_balance }}</v-ons-text></td>
                  </td>
                  <td>
                  </td>
                </tr>
                <tr>
                  <td colspan="3">
                  </td>
                  <td class="text-right"><v-ons-text>Discount</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ stockin.discount }}</v-ons-text></td>
                  <td></td>
                </tr>
                <tr>
                  <td colspan="3">

                  </td>
                  <td class="text-right"><v-ons-text>Total</v-ons-text></td>
                  <td class="text-right"><v-ons-text>{{ stockin.total }}</v-ons-text></td>
                  <td></td>
                </tr>
              </tfoot>
            </table>
          </div>
    </v-ons-row>
  </v-ons-card>
  <!-- <img :src="imgLink + stockin.image" style="width: 125px"> -->
  <v-ons-fab position="bottom right" @click="StockInEdit(animation, stockin)">
			<v-ons-icon icon="md-edit"></v-ons-icon>
		</v-ons-fab>
</v-ons-page>
</template>

<script type="text/javascript">
import Auth from '../../../store/auth'
// import Flash from '../../../helpers/flash'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import StockInForm from './Form.vue'
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
      imgLink: imgUrl + 'stockins/',
      authState: Auth.state,
      animation: 'default',
      isRemoving: false,
      isProcessing: false,
      stockinData: {
        supplier: {},
        itemins: []
      }
    }
  },
  created() {
    get( apiDomain + `/stockins/${this.stockin.id}`)
      .then((res) => {
        this.stockinData = res.data.stockin
      })
  },
  methods: {
    StockInEdit(name, data) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        stockin: data,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: StockInForm,
        data() {
          return {
            animation: name,
            stockin: data,
            title: "Edit Invoice",
            meta: 'edit'
          }
        }
      });
    },
    remove() {
      this.isRemoving = false
      del( apiDomain + `/stockins/${this.stockin.id}`)
        .then((res) => {
          if (res.data.deleted) {
            // Flash.setSuccess('You have successfully deleted item!')
            this.$router.push('/stockins')
          }
        })
    }
  }
}
</script>

<style scoped>

</style>
