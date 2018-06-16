<template>
<v-ons-page>
  <custom-toolbar backLabel="Anim" :title="title"></custom-toolbar>
  <v-ons-card>
    <img :src="imgLink + customer.image" style="width: 100%">
    <v-ons-list>
      <v-ons-list-header>Detail info</v-ons-list-header>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-face" class="list-item__icon text__color"></v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ customer.name }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-check-square" class="list-item__icon text__color"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ customer.code }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-bookmark" class="list-item__icon text__color"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ customer.nrc }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-smartphone" class="list-item__icon text__color"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text  v-for="phone in customer.phones" :key="phone.id">
            {{ phone.phone}}
          </v-ons-text>
        </label>
      </v-ons-list-item>
    </v-ons-list>
    <br>
    <v-ons-list>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-plus-square" class="list-item__icon text__color"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ customer.company }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-email" class="list-item__icon text__color"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ customer.email }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="md-pin" class="list-item__icon text__color"> </v-ons-icon>
        </div>
        <label class="center">
          <v-ons-text>
            {{ customer.address }}
          </v-ons-text>
        </label>
      </v-ons-list-item>
      <!-- <v-ons-list-item>
        <div class="right">
          <img :src="imgLink + customer.image" style="width: 125px">
        </div>
      </v-ons-list-item> -->
    </v-ons-list>
  </v-ons-card>
  <v-ons-fab position="bottom right" @click="EditCustomer(animation, customer)">
			<v-ons-icon icon="md-edit"></v-ons-icon>
		</v-ons-fab>
</v-ons-page>
</template>

<script type="text/javascript">
import Auth from '../../../store/auth'
// import Flash from '../../../helpers/flash'
import { get, del, apiDomain, imgUrl } from '../../../helpers/api'
import CustomerEdit from './Edit.vue'
// import ItemIn from './ItemIn.vue'
// import ItemOut from './ItemOut.vue'
export default {
  data() {
    return {
      state: 'initial',
      ratio: 0,
      animation: 'default',
      imgLink: imgUrl + 'customers/',
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
      del( apiDomain + `/customers/${this.customer.id}`)
        .then((res) => {
          if (res.data.deleted) {
            // Flash.setSuccess('You have successfully deleted item!')
            // this.$router.push('/customers')
          }
        })
    },
    EditCustomer(name, data) {
      this.$store.commit('navigator/options', {
        // Sets animations
        animation: name,
        form: data,
        // Resets default options
        callback: () => this.$store.commit('navigator/options', {})
      });

      this.$store.commit('navigator/push', {
        extends: CustomerEdit,
        data() {
          return {
            animation: name,
            form: data,
            title: "Edit Customer",
          }
        }
      });
    }
  }
}
</script>

<style scoped>

</style>
