<template>
  <v-ons-page>
    <v-ons-row>
      <v-ons-col width="50%">
        <v-ons-card @click="stockInPush(animation, stockIns)">
          <v-ons-icon icon="fa-indent" size="50px" class="text__color" style="line-height:normal; text-align:center"></v-ons-icon>
          <div class="content card__content card--material__content">ကုန္ပစၥည္း အ၀င္ (အ၀ယ္)</div>
        </v-ons-card>
      </v-ons-col>
      <v-ons-col width="50%">
        <v-ons-card @click="stockOutPush(animation, stockOuts)">
          <v-ons-icon icon="fa-outdent" size="50px" class="text__color" style="line-height:normal; text-align:center"></v-ons-icon>
          <div class="content card__content card--material__content">ကုန္ပစၥည္း အထြက္ (အေရာင္း)</div>
        </v-ons-card>
      </v-ons-col>
    </v-ons-row>
    <v-ons-row>
      <v-ons-col width="50%">
        <v-ons-card @click="supplierPush(animation, suppliers)">
          <v-ons-icon icon="fa-address-book" size="50px" class="text__color" style="line-height:normal; text-align:center"></v-ons-icon>
          <div class="content card__content card--material__content">Supplier List</div>
        </v-ons-card>
      </v-ons-col>
      <v-ons-col width="50%">
        <v-ons-card @click="customerPush(animation, customers)">
          <v-ons-icon icon="fa-address-book" size="50px" class="text__color" style="line-height:normal; text-align:center"></v-ons-icon>
          <div class="content card__content card--material__content">Customer List</div>
        </v-ons-card>
      </v-ons-col>
    </v-ons-row>
    <v-ons-row>
      <v-ons-col width="50%">
        <v-ons-card @click="itemPush(animation, items)">
          <v-ons-icon icon="fa-list-ul" size="50px" class="text__color" style="line-height:normal; text-align:center"></v-ons-icon>
          <div class="content card__content card--material__content">ပစၥည္း စာရင္း</div>
        </v-ons-card>
      </v-ons-col>
      <v-ons-col width="50%">
        <v-ons-card @click="userGuide(animation)">
          <v-ons-icon icon="fa-question" size="50px" class="text__color" style="line-height:normal; text-align:center"></v-ons-icon>
          <div class="content card__content card--material__content">User Guide</div>
        </v-ons-card>
      </v-ons-col>
    </v-ons-row>
  </v-ons-page>
</template>

<script>
import ItemIndex from '../views/POS/Item/Index.vue';
import CustomerIndex from '../views/POS/Customer/Index.vue';
import SupplierIndex from '../views/POS/Supplier/Index.vue';
import StockIn from '../views/POS/StockIn/Index.vue';
import StockOut from '../views/POS/StockOut/Index.vue';
import UserGuide from '../views/UserGuide/Index.vue';
import { get, apiDomain, imgUrl } from '../helpers/api';

export default {
  data () {
    return {
      animation: 'default',
      stockIns: apiDomain + `/stockins`,
      stockOuts: apiDomain + `/stockouts`,
      items: [],
      customers: [],
      suppliers: [],
    };
  },
  created() {
    get( apiDomain + `/items`)
      .then((res) => {
        this.items = res.data.items
      })
    get( apiDomain + `/customers`)
      .then((res) => {
        this.customers = res.data.customers
      })
    get( apiDomain + `/suppliers`)
      .then((res) => {
        this.suppliers = res.data.suppliers
      })
  },
  methods: {
    itemPush(name, data) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        items: data,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: ItemIndex,
        data() {
          return {
            animation: name,
            items: data,
            title: "Item List"
          }
        }
      });
    },
    customerPush(name, data) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        customers: data,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: CustomerIndex,
        data() {
          return {
            animation: name,
            customers: data,
            title: "Customer List"
          }
        }
      });
    },
    supplierPush(name, data) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        suppliers: data,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: SupplierIndex,
        data() {
          return {
            animation: name,
            suppliers: data,
            title: "Supplier List"
          }
        }
      });
    },
    stockInPush(name, stockIns) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        source: stockIns,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: StockIn,
        data() {
          return {
            animation: name,
            source: stockIns
          }
        }
      });
    },
    stockOutPush(name, stockOuts) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        source: stockOuts,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: StockOut,
        data() {
          return {
            animation: name,
            source: stockOuts
          }
        }
      });
    },
    userGuide(name) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: UserGuide,
        data() {
          return {
            animation: name
          }
        }
      });
    }
  }
};
</script>

<style>
.content {
  font-size: 14px;
  text-align: center;
}
</style>
