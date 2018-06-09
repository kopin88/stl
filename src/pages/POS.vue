<template>
  <v-ons-page>
    <v-ons-card @click="stockInPush(animation, stockIns)">
      <div class="title">Stock In </div>
      <div class="content">Stock In details</div>
    </v-ons-card>
    <v-ons-card @click="stockOutPush(animation, stockOuts)">
      <div class="title">Stock Out</div>
      <div class="content">Stock Out details</div>
    </v-ons-card>
    <v-ons-card @click="itemPush(animation, items)">
      <div class="title">Item list</div>
      <div class="content">Item details</div>
    </v-ons-card>
    <v-ons-card @click="customerPush(animation, customers)">
      <div class="title">Customers</div>
      <div class="content">Customer details</div>
    </v-ons-card>
    <v-ons-card @click="supplierPush(animation, suppliers)">
      <div class="title">Suppliers</div>
      <div class="content">Supplier details</div>
    </v-ons-card>
  </v-ons-page>
</template>

<script>
import ItemIndex from '../views/POS/Item/Index.vue';
import CustomerIndex from '../views/POS/Customer/Index.vue';
import SupplierIndex from '../views/POS/Supplier/Index.vue';
import StockIn from '../views/POS/StockIn/Index.vue';
import StockOut from '../views/POS/StockOut/Index.vue';
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
    }
  }
};
</script>

<style>
.intro {
  text-align: left;
  padding: 0 22px;
  margin-top: 20px;
  font-size: 14px;
  line-height: 1.4;
  color: rgba(0, 0, 0, .54);
}

ons-card {
  cursor: pointer;
  color: #333;
}

.card__title, .card--material__title {
  font-size: 20px;
}
</style>
