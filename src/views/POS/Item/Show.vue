<template>
<v-ons-page>
  <page-toolbar :title="item.name"></page-toolbar>
  <v-ons-card style="margin-top:63px; padding-bottom:80px">
    <img :src="imgLink + item.image" style="width: 100%">
    <v-ons-list>
      <v-ons-list-header>Detail info</v-ons-list-header>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-assignment-check" style="color: green;" class="list-item__icon"></v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ item.type.name }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-label-alt" style="color: green;" class="list-item__icon"></v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ item.brand.name }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-face" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ item.name }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-check-square" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ item.code }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-assignment" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          {{ item.descriptions }}
        </label>
      </v-ons-list-item>
    </v-ons-list>
    <br>
    <v-ons-list>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-smartphon" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            Byu Price : {{ item.buy_price }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-smartphon" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            Sale Price : {{ item.sale_price }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-smartphon" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            Qty : {{ item.qty_total }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
    </v-ons-list>
  </v-ons-card>
  <v-ons-fab position="bottom right" @click="$router.push('/items/'+ item.id + '/edit')">
			<v-ons-icon icon="md-edit"></v-ons-icon>
		</v-ons-fab>
</v-ons-page>
</template>

<script type="text/javascript">
import Auth from '../../../store/auth'
// import Flash from '../../../helpers/flash'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import PageToolbar from '../../Layout/Toolbar.vue'
// import ItemIn from './ItemIn.vue'
// import ItemOut from './ItemOut.vue'
export default {
  components: {
    // ItemIn, ItemOut,
    PageToolbar
  },
  data() {
    return {
      // backLink: 'items',
      imgLink: imgUrl + 'items/',
      authState: Auth.state,
      isRemoving: false,
      isProcessing: false,
      item: {
        brand: {},
        type: {}
      },
			// item: {}
    }
  },
  created() {
    get( apiDomain + `/items/${this.$route.params.id}`)
      .then((res) => {
        this.item = res.data.item
      })
  },
  methods: {
    remove() {
      this.isRemoving = false
      del( apiDomain + `/items/${this.$route.params.id}`)
        .then((res) => {
          if (res.data.deleted) {
            // Flash.setSuccess('You have successfully deleted item!')
            this.$router.push('/items')
          }
        })
    }
  }
}
</script>

<style scoped>

</style>
