<template>
  <v-ons-page>
    <search-toolbar backLabel="Anim">
      <div class="center">
        <input type="text"
               id="search"
               class="search"
               placeholder=" search item .."
               v-model="searchquery"
               v-on:keyup="autoComplete"
               autofocus>
      </div>
      <div class="right">
      </div>
    </search-toolbar>
    <v-ons-list>
      <v-ons-list-item v-if="items.length == 0">
        no item ...
      </v-ons-list-item>

      <v-ons-list-item v-else v-for="item in items" :key="item.id"
          @click="transition(animation, item)"
          modifier="chevron"
          class="list-item-container">
      <div class="left">
        <img class="list-item__thumbnail" :src="imgLink + item.image" >
      </div>
      <div class="center">
        <span class="list-item__title">{{ item.name }}</span>
        <span class="list-item__subtitle">code : {{ item.code }}</span>
        <span class="list-item__subtitle">price : {{ item.sale_price }}</span>
      </div>
      <div class="right">
        {{item.qty_total}}
      </div>
      </v-ons-list-item>
    </v-ons-list>
  </v-ons-page>
</template>
<script type="text/javascript">
import Auth from '../../../store/auth'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import ItemEdit from './Edit.vue'
import axios from 'axios'
import ItemShow from './Show.vue'
export default {
  data() {
    return {
      state: 'initial',
      ratio: 0,
      animation: 'default',
      imgLink: imgUrl + 'items/',
      // authState: Auth.state,
      // isRemoving: false,
      // isProcessing: false,
      searchquery: '',
      items: []
    }
  },
  methods: {
    autoComplete(){
        this.items = [];
        if(this.searchquery.length >= 2){
         axios.get('http://stl.wanyumm.com/api/search-items',{params: {searchquery: this.searchquery}}).then(response => {
            console.log(response);
          this.items = response.data;
         });
        }
      },
      transition(name, data) {
        this.$store.commit('navigator/options', {
          // Sets animations
          animation: name,
          item: data,
          // Resets default options
          callback: () => this.$store.commit('navigator/options', {})
        });

        this.$store.commit('navigator/replace', {
          extends: ItemShow,
          data() {
            return {
              animation: name,
              item: data,
              title: data.name,
            }
          }
        });
      },

  }
}
</script>

<style scoped>
.search {
    height: 35px;
    box-sizing: border-box;
    border: 0px solid ;/*rgba(250,250,250,0.38);    */
    border-radius: 4px;
    font-size: 18px;
    color:black;
    background: transparent;
    /* background-image: url(""); */
    /* background-size: 17px; */
    /* background-position: 1px 1px; */
    /* background-repeat: no-repeat; */
    /* padding: 12px 20px 12px 40px; */
    -webkit-transition: width 0.4s ease-in-out;
    transition: width 0.4s ease-in-out;
    z-index: -1;
}

.search:focus {
    width: 100%;
    border: 1px solid rgba(250,250,250,0.38);
    background-color: white;
}

</style>
