<template>
<v-ons-page>
  <page-toolbar :title="title"></page-toolbar>
    <v-ons-card style="margin-top:63px;">
      <image-upload v-model="form.image"></image-upload>
      <v-ons-text class="text__danger" v-if="error.image">{{"ပံု ထည့္ပါ"}}</v-ons-text>
        <v-ons-list>
          <v-ons-list-header>Detail info</v-ons-list-header>
          <!-- For Type  -->
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-assignment-check" style="color: green;" class="list-item__icon"></v-ons-icon>
            </div>
            <div class="center">
              <v-ons-select style="width: 90%" v-model="form.type_id">
                <option value="" disabled selected>Select Type</option>
                <option v-for="type in option.types" :value="type.id">
                  {{type.name}}
                </option>
              </v-ons-select>
              <v-ons-text class="text__danger" v-if="error.type_id">{{"Type ေရြးပါ"}}</v-ons-text>
            </div>
            <div class="right">
              <!-- <span class="input-group-addon"> -->
                <v-ons-text @click="addType = !addType">
                  <v-ons-icon icon="md-plus" class="list-item__icon" v-if="!addType"> </v-ons-icon>
                  <v-ons-icon icon="md-chevron-down" class="list-item__icon" v-if="addType"> </v-ons-icon>
                </v-ons-text>
            </div>
          </v-ons-list-item>
          <v-ons-alert-dialog modifier="rowfooter" :visible.sync="addType">
            <span slot="title">Create Type</span>
            <v-ons-input float placeholder="Type Name" v-model="type.name" style="width:100%">
            </v-ons-input>
            <v-ons-input float placeholder="Type Description" v-model="type.descriptions" style="width:100%; margin-top:30px">
            </v-ons-input>
            <template slot="footer">
              <div class="alert-dialog-button" @click="addType = false">Cancel</div>
              <div class="alert-dialog-button"  @click="saveType" :disabled="isProcessingType">Save</div>
            </template>
          </v-ons-alert-dialog>

          <!-- For Brand -->
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-label-alt" style="color: green;" class="list-item__icon"></v-ons-icon>
            </div>
            <div class="center">
              <v-ons-select style="width: 90%" v-model="form.brand_id">
                <option value="" disabled selected>Select Brand</option>
                <option v-for="brand in option.brands" :value="brand.id">
                  {{brand.name}}
                </option>
              </v-ons-select>
              <v-ons-text class="text__danger" v-if="error.brand_id">{{"Brand ေရြးပါ"}}</v-ons-text>
            </div>
            <div class="right">
              <!-- <span class="input-group-addon"> -->
                <v-ons-text @click="addBrand = !addBrand">
                  <v-ons-icon icon="md-plus" class="list-item__icon" v-if="!addBrand"> </v-ons-icon>
                  <v-ons-icon icon="md-chevron-down" class="list-item__icon" v-if="addBrand"> </v-ons-icon>
                </v-ons-text>
            </div>
          </v-ons-list-item>
          <v-ons-alert-dialog modifier="rowfooter" :visible.sync="addBrand">
            <span slot="title">Create Brand</span>
            <v-ons-input float placeholder="Brand Name" v-model="brand.name" style="width:100%">
            </v-ons-input>
            <v-ons-input float placeholder="Brand Description" v-model="brand.descriptions" style="width:100%; margin-top:30px">
            </v-ons-input>
            <template slot="footer">
              <div class="alert-dialog-button" @click="addBrand = false">Cancel</div>
              <div class="alert-dialog-button"  @click="saveBrand" :disabled="isProcessingBrand">Save</div>
            </template>
          </v-ons-alert-dialog>

          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-face" style="color: green;" class="list-item__icon"></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Name" v-model="form.name" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.name">{{"Name ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-check-square" style="color: green;" class="list-item__icon"> </v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Code" v-model="form.code" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.code">{{"Code ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-assignment" style="color: green;" class="list-item__icon"> </v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Description" v-model="form.descriptions" style="width:100%">
              </v-ons-input>
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
              <v-ons-input float placeholder="Byu Price" v-model="form.byu_price" style="width:100%" v-if="editBuyPrice">
              </v-ons-input>
              <v-ons-text>
                <label for="" v-if="!editBuyPrice">Buy Price : {{ form.sale_price }}</label>
              </v-ons-text>
            </label>
            <label class="right">
              <v-ons-icon icon="md-edit" style="color: green;" class="list-item__icon" @click="editBuyPrice = !editBuyPrice" v-if="!editBuyPrice"> </v-ons-icon>
              <v-ons-icon icon="md-check-square" style="color: green;" class="list-item__icon" @click="editBuyPrice = !editBuyPrice" v-if="editBuyPrice"> </v-ons-icon>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-smartphon" style="color: green;" class="list-item__icon"> </v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Sale Price" v-model="form.sale_price" style="width:100%" v-if="editSalePrice">
              </v-ons-input>
              <v-ons-text>
                <label for="" v-if="!editSalePrice">Sale Price : {{ form.sale_price }}</label>
              </v-ons-text>
            </label>
            <label class="right">
              <v-ons-icon icon="md-edit" style="color: green;" class="list-item__icon" @click="editSalePrice = !editSalePrice" v-if="!editSalePrice"> </v-ons-icon>
              <v-ons-icon icon="md-check-square" style="color: green;" class="list-item__icon" @click="editSalePrice = !editSalePrice" v-if="editSalePrice"> </v-ons-icon>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-smartphon" style="color: green;" class="list-item__icon"> </v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="QTY" v-model="form.qty_total" style="width:100%" v-if="editQTY">
              </v-ons-input>
              <v-ons-text>
                <label for="" v-if="!editQTY">QTY : {{ form.qty_total }}</label>
              </v-ons-text>
              <v-ons-text class="text__danger" v-if="error.qty_total">{{" '0' ထည့္ေပးပါ"}}</v-ons-text>
            </label>
            <label class="right">
              <v-ons-icon icon="md-edit" style="color: green;" class="list-item__icon" @click="editQTY = !editQTY" v-if="!editQTY"> </v-ons-icon>
              <v-ons-icon icon="md-check-square" style="color: green;" class="list-item__icon" @click="editQTY = !editQTY" v-if="editQTY"> </v-ons-icon>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="center">
              <v-ons-button @click="$router.back()" :disabled="isProcessing">Cancel</v-ons-button>
            </div>
            <div class="right">
              <v-ons-button @click="save" :disabled="isProcessing">Save</v-ons-button>
            </div>
          </v-ons-list-item>
        </v-ons-list>
    </v-ons-card>
</v-ons-page>
</template>
<script>
import Vue from 'vue'
// import Flash from '../../../helpers/flash'
import { get, post, apiDomain, imgUrl} from '../../../helpers/api'
import { toMulipartedForm } from '../../../helpers/form'
import ImageUpload from '../../../components/ImageUpload.vue'
import PageToolbar from '../../Layout/Toolbar.vue'

// import { toMulipartedForm } from '../../../helpers/form'
export default {
  components: {
    ImageUpload,
    PageToolbar
  },
  data() {
    return {
      title: 'Create Item',
      addType: false,
      addBrand: false,
      editQTY: false,
      editBuyPrice: false,
      editSalePrice: false,
      form: {},
      type: { name: '' },
      brand: { name: '' },
      option: {
        'types': [],
        'brands': []
      },
      error: {},
      isProcessing: false,
      isProcessingType: false,
      isProcessingBrand: false,
      imgLink: imgUrl + 'images/items/',

      initializeURL: apiDomain + `/items/create`,
      storeURL: apiDomain + `/items`,
      typeURL: apiDomain + `/types`,
      brandURL: apiDomain + `/brands`,
      action: 'Create'
    }
  },
  created() {
    if (this.$route.meta.mode === 'edit') {
      this.initializeURL = apiDomain + `/items/${this.$route.params.id}/edit`
      this.storeURL = apiDomain + `/items/${this.$route.params.id}?_method=PUT`
      this.action = 'Update'
    }
    get(this.initializeURL)
      .then((res) => {
        Vue.set(this.$data, 'form', res.data.form)
        Vue.set(this.$data, 'type', res.data.type)
        Vue.set(this.$data, 'brand', res.data.brand)
        Vue.set(this.$data, 'option', res.data.option)
      })
  },

  methods: {
    save() {
      const form = toMulipartedForm(this.form, this.$route.meta.mode)
      post(this.storeURL, form)
        .then((res) => {
          if (res.data.saved) {
            // Flash.setSuccess(res.data.message)
            // this.$router.push(`/items/${res.data.id}`)
            this.$router.push(`/items`)
          }
          this.isProcessing = false
        })
        .catch((err) => {
          if (err.response.status === 422) {
            this.error = err.response.data
          }
          this.isProcessing = false
        })
    },
    saveType() {
      const type = toMulipartedForm(this.type, this.$route.meta.mode)
      post(this.typeURL, type)
        .then((res) => {
          if (res.data.saved) {
            // Flash.setSuccess(res.data.message)
          }
          this.type.name = "",
          this.addType = false
          this.option.types.push(res.data.type)
          this.isProcessingType = false
        })
        .catch((err) => {
          if (err.response.status === 322) {
            this.error = err.response.data
          }
          this.isProcessingType = false
        })
    },
    saveBrand() {
      const brand = toMulipartedForm(this.brand, this.$route.meta.mode)
      post(this.brandURL, brand)
        .then((res) => {
          if (res.data.saved) {
            // Flash.setSuccess(res.data.message)
            // this.$router.push(`/items/${res.data.id}`)
          }
          this.brand.name = ""
          this.addBrand = false
          this.option.brands.push(res.data.brand)
          this.isProcessingBrand = false
        })
        .catch((err) => {
          if (err.response.status === 322) {
            this.error = err.response.data
          }
          this.isProcessingBrand = false
        })
    }
  }
}
</script>

<style scoped>

</style>
