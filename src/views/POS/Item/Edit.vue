<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="title">
      <template slot="right" v-if="(form.name != '') && (form.code != '') && (form.type_id != '') && (form.brand_id != '') && (form.descriptions != '') && (form.image != '')">
        <v-ons-icon style="color:white" icon="md-check" :disabled="isProcessing" @click="save"></v-ons-icon>
      </template>
    </custom-toolbar>
    <v-ons-card>
        <v-ons-list>

          <v-ons-list-header>Detail info</v-ons-list-header>

          <v-ons-list-item :modifier="md ? 'nodivider' : ''">
            <div class="left">
              <v-ons-icon v-if="form.name != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
              <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input type="text" placeholder="Item Name" v-model="form.name" float style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.name">{{ error.name[0] }}
              </v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item :modifier="md ? 'nodivider' : ''">
            <div class="left">
              <v-ons-icon v-if="form.code != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
              <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input type="text" placeholder="Code" v-model="form.code" float style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.code">{{ error.code[0] }}
              </v-ons-text>
            </label>
          </v-ons-list-item>

          <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-show="!addType">
            <div class="left">
              <v-ons-icon v-if="form.type_id != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
              <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
            </div>
            <label class="center">
              <select class="form-control" style="width: 95%" v-model="form.type_id">
                <option value="" disabled selected><label>Type ...</label></option>
                <option v-for="type in option.types" :value="type.id">
                  {{ type.name }}
                </option>
              </select>
            </label>
            <div class="right">
              <!-- <ion-icon name="add-circle"></ion-icon> -->
              <v-ons-icon size="30px" icon="md-plus-square" class="list-item__icon text__color" @click="newType"></v-ons-icon>
            </div>
          </v-ons-list-item>
          <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-show="addType">
            <div class="left">
              <v-ons-icon v-if="type.name != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
              <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input type="text" placeholder="Add Type" float style="width:95%;font-family:Zawgyi-One;" v-focus="typefocus" @focus="typefocus = true" @blur="typefocus = false" v-model="type.name"> </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.name">{{ error.name[0] }}
              </v-ons-text>
            </label>
            <div class="right">
              <v-ons-icon size="30px" icon="md-check-square" class="list-item__icon text__color" @click="saveType" :disabled="isProcessingType"></v-ons-icon>
            </div>
          </v-ons-list-item>

          <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-show="!addBrand">
            <div class="left">
              <v-ons-icon v-if="form.brand_id != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
              <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
            </div>
            <label class="center">
              <select class="form-control" style="width: 95%" v-model="form.brand_id">
                <option value="" disabled selected><label>Brand ...</label></option>
                <option v-for="brand in option.brands" :value="brand.id">
                  {{ brand.name }}
                </option>
              </select>
            </label>
            <div class="right">
              <v-ons-icon size="30px" icon="md-plus-square" class="list-item__icon text__color" @click="newBrand"></v-ons-icon>
            </div>
          </v-ons-list-item>
          <v-ons-list-item :modifier="md ? 'nodivider' : ''" v-show="addBrand">
            <div class="left">
              <v-ons-icon v-if="brand.name != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
              <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input type="text" placeholder="Add Brand" float style="width:95%;font-family:Zawgyi-One;" v-focus="brandfocus" @focus="brandfocus = true" @blur="brandfocus = false" v-model="brand.name"> </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.name">{{ error.name[0] }}
              </v-ons-text>
            </label>
            <div class="right">
              <v-ons-icon size="30px" icon="md-check-square" class="list-item__icon text__color" :disabled="isProcessingBrand" @click="saveBrand"></v-ons-icon>
            </div>
          </v-ons-list-item>
          <v-ons-list-item :modifier="md ? 'nodivider' : ''">
            <div class="left">
              <v-ons-icon v-if="form.descriptions != ''" icon="md-label-alt" class="list-item__icon text__color"></v-ons-icon>
              <v-ons-icon v-else icon="md-label-alt" class="list-item__icon"></v-ons-icon>
            </div>
            <label class="center">
                <v-ons-input type="text" placeholder="Description" v-model="form.descriptions" float style="width:90%;font-family:Zawgyi-One;"> </v-ons-input>
                <v-ons-text class="text__danger" v-if="error.descriptions">{{ error.descriptions[0] }}
                </v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <image-upload v-model="form.image" :forderPath="forderPath" style="width:100%"></image-upload>
            <v-ons-text class="text__danger" v-if="error.image">{{"ပံု ထည့္ပါ"}}</v-ons-text>
          </v-ons-list-item>
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
import { focus } from 'vue-focus';
import ItemShow from './Show.vue'

export default {
  directives: { focus: focus },
  components: { ImageUpload },
  data() {
    return {
      // title: 'Create Item',
      addType: false,
      addBrand: false,
      typefocus: false,
      brandfocus: false,
      editQTY: false,
      editBuyPrice: false,
      editSalePrice: false,
      type: { name: '' },
      forderPath:'items/',
      brand: { name: '' },
      option: {
        'types': [],
        'brands': []
      },
      error: {},
      modalVisible: false,
      isProcessing: false,
      isProcessingType: false,
      isProcessingBrand: false,
      imgLink: imgUrl + 'images/items/',

      storeTypeURL: apiDomain + `/types`,
      storeBrandURL: apiDomain + `/brands`,
      action: 'Edit'
    }
  },
  created() {
      get(apiDomain + `/items/${this.form.id}/edit`)
        .then((res) => {
          Vue.set(this.$data, 'option', res.data.option)
        })
  },
  methods: {
    newType() {
      this.addType = !this.addType
      this.typefocus = true
    },
    newBrand() {
      this.addBrand = !this.addBrand
      this.brandfocus = true
    },
    save() {
      this.modalVisible = true
      this.isProcessing = true
      const form = toMulipartedForm(this.form, 'edit')
      post(apiDomain + `/items/${this.form.id}?_method=PUT`, form)
          .then((res) => {
              // const apiUrl = apiDomain
              if(res.data.saved) {
                this.$store.commit('navigator/pop', {
                  extends: ItemShow,
                  data() {
                    return {
                      item: res.data.item,
                      title: res.data.item.name,
                      // reportsource: `items/${res.data.item.year_id}/reports/${res.data.item.id}`
                    }
                  }
                })
              }
              this.isProcessing = false
          })
    },
    newType() {
      this.addType = !this.addType
      this.typefocus = true
    },
    saveType() {
      // this.isProcessingType = true
      const type = toMulipartedForm(this.type, 'create')
      post(this.storeTypeURL, type)
      .then((res) => {
        if(res.data.saved) {
          this.type.name = "",
          this.addType = false
          this.option.types.push(res.data.type)
          this.isProcessingType = false
        }
        this.isProcessing = false
      })
    },
    newBrand() {
      this.addBrand = !this.addBrand
      this.brandfocus = true
    },
    saveBrand() {
      this.isProcessingBrand = true
      const brand = toMulipartedForm(this.brand, 'create')
      post(this.storeBrandURL, brand)
      .then((res) => {
        if(res.data.saved) {
          this.brand.name = ""
          this.addBrand = false
          this.option.brands.push(res.data.brand)
          this.isProcessingBrand = false
        }
        this.isProcessing = false
      })
    },
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
