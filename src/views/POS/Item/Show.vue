<template>
<v-ons-page>
  <custom-toolbar backLabel="Anim" :title="title"></custom-toolbar>
  <v-ons-card>
    <v-ons-list>
      <v-ons-list-header>Item detail info</v-ons-list-header>

      <v-ons-list-item>
        <div class="left">
          <v-ons-text>Name :</v-ons-text>
        </div>
        <div class="center">
          {{item.name}}
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-text>Code :</v-ons-text>
        </div>
        <div class="center">
          {{item.code}}
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-text>Type :</v-ons-text>
        </div>
        <div class="center">
          {{item.type.name}}
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-text>Brand :</v-ons-text>
        </div>
        <div class="center">
          {{item.brand.name}}
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-text>Description</v-ons-text>
        </div>
        <div class="center">
          {{item.descriptions}}
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-text>Buy Price :</v-ons-text>
        </div>
        <div class="center">
          {{item.buy_price}}
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-text>Sale Price :</v-ons-text>
        </div>
        <div class="center">
          {{item.sale_price}}
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-text>Balance :</v-ons-text>
        </div>
        <div class="center">
          {{item.qty_total}}
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>

      <v-ons-list-item>
        <img :src="imgLink + item.image" style="width: 100%">
      </v-ons-list-item>
    </v-ons-list>
  </v-ons-card>
  <v-ons-fab position="bottom right" @click="EditItem(animation, item)">
			<v-ons-icon icon="md-edit"></v-ons-icon>
		</v-ons-fab>
</v-ons-page>
</template>

<script type="text/javascript">
import Auth from '../../../store/auth'
// import Flash from '../../../helpers/flash'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import ItemEdit from './Edit.vue'
// import ItemIn from './ItemIn.vue'
// import ItemOut from './ItemOut.vue'
export default {
  data() {
    return {
      state: 'initial',
      ratio: 0,
      animation: 'default',
      imgLink: imgUrl + 'items/',
      authState: Auth.state,
      isRemoving: false,
      isProcessing: false,
      item: {
        brand: {},
        type: {}
      },
    }
  },
  methods: {
    remove() {
      this.isRemoving = false
      del( apiDomain + `/items/${this.item.id}`)
        .then((res) => {
          if (res.data.deleted) {
            // Flash.setSuccess('You have successfully deleted item!')
            // this.$router.push('/items')
          }
        })
    },
    EditItem(name, data) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        form: data,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: ItemEdit,
        data() {
          return {
            animation: name,
            form: data,
            meta: 'edit',
            title: "Edit Item",
          }
        }
      });
    }
  }
}
</script>

<style scoped>

</style>
