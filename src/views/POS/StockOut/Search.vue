<template>
  <v-ons-page>
    <search-toolbar backLabel="Anim">
      <template slot="center">
        <input type="text"
        style="color:white"
        id="search"
        class="search"
        placeholder="  Search stock  out"
        v-model="searchquery"
        v-on:keyup="autoComplete"
        v-focus="searchfocus"
        @focus="searchfocus = true"
        @blur="searchfocus = false"
        >
      </template>
      <template slot="right">
      <v-ons-icon  style="color:white" icon="md-close"
        v-if="searchquery != ''"
        @click="searchClear">
      </v-ons-icon>
        <v-ons-icon style="color:white" icon="md-search"
          v-if="searchquery == ''"
          @click="searchStockOut">
        </v-ons-icon>
      </template>
    </search-toolbar>
    <v-ons-list>
      <v-ons-list-item v-if="searchquery.length < 2">
        Search from stock out 'date' or 'inovice' ..
      </v-ons-list-item>
      <v-ons-list-item v-if="(stockouts.length == 0) && (status == 200) && (searchquery.length >= 2)">
        no stock in ...
      </v-ons-list-item>

      <v-ons-list-item v-else v-for="stockout in stockouts" :key="stockout.id"
          @click="transition(animation, stockout)"
          modifier="chevron"
          class="list-item-container">
          <div class="left">
            {{ stockout.date | moment("DD-MMM-YYYY") }}
          </div>
          <div class="center" style="margin-left:10px">
            <span class="list-item__title">{{ stockout.invoice_no }}</span>
            <span class="list-item__subtitle">Customer : {{stockout.customer.name}}</span>
            <!-- <span class="list-item__subtitle">price : {{ stockout.sale_price }}</span> -->
          </div>
          <div class="right">
            {{ stockout.total }}
          </div>
      </v-ons-list-item>
    </v-ons-list>
  </v-ons-page>
</template>
<script>
import Auth from '../../../store/auth'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import axios from 'axios'
import StockOutShow from './Show.vue'
import { focus } from 'vue-focus';

export default {
  directives: { focus: focus },
  data() {
    return {
      state: 'initial',
      ratio: 0,
      animation: 'default',
      // imgLink: imgUrl + 'stockouts/',
      // authState: Auth.state,
      // isRemoving: false,
      // isProcessing: false,
      searchquery: '',
      stockouts: [],
      status:'',
      searchfocus: true,
    }
  },
  methods: {
    autoComplete(){
        this.stockouts = [];
        if(this.searchquery.length >= 2){
         axios.get('http://wanyumm.com/api/search-stockouts',{params: {searchquery: this.searchquery}}).then(response => {
            // console.log(response);
          this.stockouts = response.data;
          this.status = response.status;
         });
        }
      },
      searchClear() {
        this.searchfocus = false;
        this.searchquery = '';
        this.searchfocus = true;
      },
      searchStockOut() {
        this.searchfocus = true;
      },
      transition(name, data) {
        this.$store.commit('navigator/options', {
          // Sets animations
          animation: name,
          stockout: data,
          // Resets default options
          callback: () => this.$store.commit('navigator/options', {})
        });

        this.$store.commit('navigator/replace', {
          extends: StockOutShow,
          data() {
            return {
              animation: name,
              stockout: data,
              title: data.name,
            }
          }
        });
      },

  }
}
</script>

<!-- /* <style scoped>

.search {
    float: right;
    height: 100%;
    width: 30px;
    box-sizing: border-box;
    border: 0px solid ;
    rgba(250,250,250,0.38);
    border-radius: 4px;
    color:white;
    font-size: 20px;
    background-color: $theme-color;
    background: transparent;
    background-image: url("");
    background-image: url("../../../assets/icon/2x/search_white_18dp.png");
    background-size: 22px;
    margin: 0 4px 0 0;
    line-height: 56px;
    background-position: right center;
    background-repeat: no-repeat;
    padding: 0 12px;
    -webkit-transition: width 0.4s ease-in-out;
    z-index: -1;
    transition: width 0.4s ease-in-out;
}

.search:focus {
    width: 100%;
    border: 1px solid rgba(250,250,250,0.38);
    background-color: $theme-color;
    padding: 12px 40px 12px 12px;
    z-index: -1;
    margin: 0 4px 0 0;
}

::placeholder {
  color: #C8E6C9;
  font-size: 20px;
  font-weight: 500;
}

</style> */ -->
