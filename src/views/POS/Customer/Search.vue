<template>
  <v-ons-page>
    <search-toolbar backLabel="Anim">
      <template slot="center">
        <input type="text"
        style="color:white"
        id="search"
        class="search"
        placeholder="  Select Customer"
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
          @click="searchCustomer">
        </v-ons-icon>
      </template>
    </search-toolbar>
    <v-ons-list>
      <v-ons-list-item v-if="searchquery.length < 2">
        Search from item 'name' or 'code' ..
      </v-ons-list-item>
      <v-ons-list-item v-if="(customers.length == 0) && (status == 200) && (searchquery.length >= 2)">
        no item ...
      </v-ons-list-item>

      <v-ons-list-item v-else v-for="customer in customers" :key="customer.id"
          @click="transition(animation, customer)"
          modifier="chevron"
          class="list-item-container">
      <div class="left">
        <img class="list-item__thumbnail" :src="imgLink + customer.image" >
      </div>
      <div class="center">
        <span class="list-item__title">{{ customer.name }}</span>
        <span class="list-item__subtitle">code : {{ customer.code }}</span>
        <!-- <span class="list-item__subtitle">price : {{ customer.sale_price }}</span> -->
      </div>
      <div class="right">
        {{customer.rem_balance}}
      </div>
      </v-ons-list-item>
    </v-ons-list>
  </v-ons-page>
</template>
<script>
import Auth from '../../../store/auth'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import axios from 'axios'
import CustomerShow from './Show.vue'
import { focus } from 'vue-focus';

export default {
  directives: { focus: focus },
  data() {
    return {
      state: 'initial',
      ratio: 0,
      animation: 'default',
      imgLink: imgUrl + 'customers/',
      // authState: Auth.state,
      // isRemoving: false,
      // isProcessing: false,
      searchquery: '',
      customers: [],
      status:'',
      searchfocus: true,
    }
  },
  methods: {
    autoComplete(){
        this.customers = [];
        if(this.searchquery.length >= 2){
         axios.get('http://wanyumm.com/api/search-customers',{params: {searchquery: this.searchquery}}).then(response => {
            // console.log(response);
          this.customers = response.data;
          this.status = response.status;
         });
        }
      },
      searchClear() {
        this.searchfocus = false;
        this.searchquery = '';
        this.searchfocus = true;
      },
      searchCustomer() {
        this.searchfocus = true;
      },
      transition(name, data) {
        this.$store.commit('navigator/options', {
          // Sets animations
          animation: name,
          customer: data,
          // Resets default options
          callback: () => this.$store.commit('navigator/options', {})
        });

        this.$store.commit('navigator/replace', {
          extends: CustomerShow,
          data() {
            return {
              animation: name,
              customer: data,
              title: data.name,
            }
          }
        });
      },

  }
}
</script>
