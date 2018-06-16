<template>
<v-ons-page>
  <custom-toolbar backLabel="Anim" :title="title">
    <template slot="right" v-if="(form.name != '') && (form.code != '') && (form.image != '')">
      <v-ons-icon style="color:white" icon="md-check" :disabled="isProcessing" @click="save"></v-ons-icon>
    </template>
  </custom-toolbar>
    <v-ons-card>
      <image-upload v-model="form.image" style="width:100%"></image-upload>
      <v-ons-text class="text__danger" v-if="error.image">{{"ပံု ထည့္ပါ"}}</v-ons-text>
        <v-ons-list>
          <v-ons-list-header>Detail info</v-ons-list-header>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-face" class="list-item__icon text__color"></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Name" v-model="form.name" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.name">{{"Name ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-check-square" class="list-item__icon text__color"> </v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Code" v-model="form.code" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.code">{{"Code ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-bookmark" class="list-item__icon text__color"> </v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="NRC" v-model="form.nrc" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.nrc">{{"မွတ္ပံုတင္ ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-smartphone" class="list-item__icon text__color"> </v-ons-icon>
            </div>
            <label class="center" v-for="(phone, index) in form.phones">
              <v-ons-input float placeholder="Phone" v-model="phone.phone" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.phones">{{"Phone ထည့္ပါ"}}</v-ons-text>
            </label>
            <label class="right">
              <button type="button" @click="remove('phones', index)">
                <!-- <v-ons-icon icon="fa-trash-o" class="list-item__icon"> </v-ons-icon> -->
                x
              </button>
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
              <v-ons-input float placeholder="Company" v-model="form.company" style="width:100%">
              <v-ons-text class="text__danger" v-if="error.company">{{"Company ထည့္ပါ"}}</v-ons-text>
              </v-ons-input>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-email" class="list-item__icon text__color"> </v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Email" v-model="form.email" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.email">{{"Email ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-pin" class="list-item__icon text__color"> </v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Address" v-model="form.address" type="textarea" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.address">{{"လိပ္စာ ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <!-- <v-ons-list-item>
            <div class="right">
              <image-upload v-model="form.image"></image-upload>
              <v-ons-text class="text__danger" v-if="error.image">{{"ပံု ထည့္ပါ"}}</v-ons-text>
            </div>
          </v-ons-list-item> -->
        </v-ons-list>
    </v-ons-card>

    <v-ons-modal
      :visible="modalVisible"
    >
      <p style="text-align: center">
        <v-ons-progress-circular indeterminate></v-ons-progress-circular>
      </p>
    </v-ons-modal>
</v-ons-page>
</template>
<script>
import Vue from 'vue'
import { get, post, apiDomain, imgUrl} from '../../../helpers/api'
import { toMulipartedForm } from '../../../helpers/form'
import ImageUpload from '../../../components/ImageUpload.vue'

export default {
  components: { ImageUpload },
  data() {
    return {
      form: {
        name: '',
        code: '',
        image: '',
        phones: [{"phone":""}]
      },
      imgLink: imgUrl + 'images/suppliers/',
      error: {},
      isProcessing: false,
      modalVisible: false,
      storeURL: apiDomain + `/suppliers`,
      action: 'Create'
    }
  },
  methods: {
    addPhone() {
      this.form.phones.push({
        phone: ''
      })
    },
    save() {
      this.isProcessing = true
      this.modalVisible = true
      const form = toMulipartedForm(this.form, 'create')
      post(this.storeURL, form)
        .then((res) => {
          if (res.data.saved) {
            this.$store.commit('navigator/pop');
          }
          this.isProcessing = false
        })
    },
    remove(type, index) {
      if (this.form[type].length > 1) {
        this.form[type].splice(index, 1)
      }
    }
  }
}
</script>

<style scoped>
.wy-bgcolor {
  background-color: rgb(30, 136, 229);
  transition: all 0s;
  /*border-radius: 10%*/
}
</style>
