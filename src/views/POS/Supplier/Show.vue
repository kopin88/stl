<template>
<v-ons-page>
  <custom-toolbar backLabel="Anim" :title="title"></custom-toolbar>
  <v-ons-card>
    <img :src="imgLink + supplier.image" style="width: 100%">
    <v-ons-list>
      <v-ons-list-header>Detail info</v-ons-list-header>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-face" style="color: green;" class="list-item__icon"></v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ supplier.name }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-check-square" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ supplier.code }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-bookmark" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ supplier.nrc }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-smartphone" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text  v-for="phone in supplier.phones" :key="phone.id">
            {{ phone.phone}}
          </v-ons-text>
        </label>
      </v-ons-list-item>
    </v-ons-list>
    <br>
    <v-ons-list>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-plus-square" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ supplier.company }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-email" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ supplier.email }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-pin" style="color: green;" class="list-item__icon"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ supplier.address }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <!-- <v-ons-list-item>
        <div class="right">
          <img :src="imgLink + supplier.image" style="width: 125px">
        </div>
      </v-ons-list-item> -->
    </v-ons-list>
  </v-ons-card>
  <v-ons-fab position="bottom right" @click="EditSupplier(animation, supplier)">
			<v-ons-icon icon="md-edit"></v-ons-icon>
		</v-ons-fab>
</v-ons-page>
</template>

<script type="text/javascript">
import Auth from '../../../store/auth'
// import Flash from '../../../helpers/flash'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import SupplierEdit from './Edit.vue'
// import ItemIn from './ItemIn.vue'
// import ItemOut from './ItemOut.vue'
export default {
  data() {
    return {
      state: 'initial',
      ratio: 0,
      animation: 'default',
      imgLink: imgUrl + 'suppliers/',
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
      del( apiDomain + `/suppliers/${this.supplier.id}`)
        .then((res) => {
          if (res.data.deleted) {
            // Flash.setSuccess('You have successfully deleted item!')
            // this.$router.push('/suppliers')
          }
        })
    },
    EditSupplier(name, data) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        form: data,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: SupplierEdit,
        data() {
          return {
            animation: name,
            form: data,
            title: "Edit Supplier",
          }
        }
      });
    }
  }
}
</script>

<style scoped>

</style>
