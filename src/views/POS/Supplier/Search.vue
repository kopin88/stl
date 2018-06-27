<template>
  <v-ons-page>
    <search-toolbar backLabel="Anim">
      <template slot="center">
        <input type="text"
        style="color:white"
        id="search"
        class="search"
        placeholder="  Search Supplier"
        v-model="searchquery"
        v-on:keyup="autoComplete"
        v-focus="searchfocus"
        @focus="searchfocus = true"
        @blur="searchfocus = false"
        >
      </template>
      <template slot="right">

        <!-- <v-ons-button class="sm-icon-button md-dense md-primary">
        <md-icon>person</md-icon>
    </v-ons-button> -->
      <v-ons-icon  style="color:white" icon="md-close"
        v-if="searchquery != ''"
        @click="searchClear">
      </v-ons-icon>
        <v-ons-icon style="color:white" icon="md-search"
          v-if="searchquery == ''"
          @click="searchSupplier">
        </v-ons-icon>
      </template>
    </search-toolbar>
    <v-ons-list>
      <v-ons-list-item v-if="searchquery.length < 2">
        Search from item 'name' or 'code' ..
      </v-ons-list-item>
      <v-ons-list-item v-if="(suppliers.length == 0) && (status == 200) && (searchquery.length >= 2)">
        no item ...
      </v-ons-list-item>

      <v-ons-list-item v-else v-for="supplier in suppliers" :key="supplier.id"
          @click="transition(animation, supplier)"
          modifier="chevron"
          class="list-item-container">
      <div class="left">
        <img class="list-item__thumbnail" :src="imgLink + supplier.image" >
      </div>
      <div class="center">
        <span class="list-item__title">{{ supplier.name }}</span>
        <span class="list-item__subtitle">code : {{ supplier.code }}</span>
        <!-- <span class="list-item__subtitle">price : {{ supplier.sale_price }}</span> -->
      </div>
      <div class="right">
        {{supplier.rem_balance}}
      </div>
      </v-ons-list-item>
    </v-ons-list>
  </v-ons-page>
</template>
<script>
import Auth from '../../../store/auth'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import axios from 'axios'
import SupplierShow from './Show.vue'
import { focus } from 'vue-focus';

export default {
  directives: { focus: focus },
  data() {
    return {
      state: 'initial',
      ratio: 0,
      animation: 'default',
      imgLink: imgUrl + 'suppliers/',
      // authState: Auth.state,
      // isRemoving: false,
      // isProcessing: false,
      searchquery: '',
      suppliers: [],
      status:'',
      searchfocus: true,
    }
  },
  methods: {
    autoComplete(){
        this.suppliers = [];
        if(this.searchquery.length >= 2){
         axios.get('http://wanyumm.com/api/search-suppliers',{params: {searchquery: this.searchquery}}).then(response => {
            // console.log(response);
          this.suppliers = response.data;
          this.status = response.status;
         });
        }
      },
      searchClear() {
        this.searchfocus = false;
        this.searchquery = '';
        this.searchfocus = true;
      },
      searchSupplier() {
        this.searchfocus = true;
      },
      transition(name, data) {
        this.$store.commit('navigator/options', {
          // Sets animations
          animation: name,
          supplier: data,
          // Resets default options
          callback: () => this.$store.commit('navigator/options', {})
        });

        this.$store.commit('navigator/replace', {
          extends: SupplierShow,
          data() {
            return {
              animation: name,
              supplier: data,
              title: data.name,
            }
          }
        });
      },

  }
}
</script>
