<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="title">
      <template slot="right">
        <v-ons-icon style="color:white" icon="md-check" :disabled="isProcessing" @click="save"></v-ons-icon>
      </template>
    </custom-toolbar>
    <v-ons-card>
        <v-ons-list>
          <v-ons-list-header>Detail info</v-ons-list-header>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-minus-circle-outline" style="color: #FB8C00;" class="list-item__icon" v-if="form.invoice_no == ''"></v-ons-icon>
              <v-ons-icon icon="md-check" style="color: green;" class="list-item__icon" v-else></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input float placeholder="Invoice No." v-model="form.invoice_no" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.invoice_no">{{"Invoice No. ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-minus-circle-outline" style="color: #FB8C00;" class="list-item__icon" v-if="selectYear == ''"></v-ons-icon>
              <v-ons-icon icon="md-check" style="color: green;" class="list-item__icon" v-else></v-ons-icon>
            </div>
            <div class="center">
              <select style="width: 100%" v-model="selectYear" class="form-control">
                <option value="" disabled selected>ဘ႑ာေရးႏွစ္ ေရြးပါ</option>
                <option v-for="year in years" :value="year.id" :selected="form.year_id">
									{{year.name}}
								</option>
              </select>
              <v-ons-text class="text__danger" v-if="error.year_id">{{"ဘ႑ာေရးႏွစ္ ေရြးပါ"}}</v-ons-text>
            </div>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-minus-circle-outline" style="color: #FB8C00;" class="list-item__icon" v-if="form.month_id == ''"></v-ons-icon>
              <v-ons-icon icon="md-check" style="color: green;" class="list-item__icon" v-else></v-ons-icon>
            </div>
            <div class="center">
              <select style="width: 100%" v-model="form.month_id" class="form-control">
                <option value="" disabled selected><label>လ ေရြးပါ ...</label></option>
								<option value="" disabled><label>ဘ႑ာေရးႏွစ္ အရင္ေရြးပါ ... </label></option>
								<option v-for="month in months" :value="month.id">
									{{month.name}}
								</option>
              </select>
              <v-ons-text class="text__danger" v-if="error.month_id">{{"ဘ႑ာေရးလ ေရြးပါ"}}</v-ons-text>
            </div>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-minus-circle-outline" style="color: #FB8C00;" class="list-item__icon" v-if="form.date == ''"></v-ons-icon>
              <v-ons-icon icon="md-check" style="color: green;" class="list-item__icon" v-else></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input type="date" float placeholder="ေန႔စြဲ" v-model="form.date" style="width:100%">
              </v-ons-input>
              <v-ons-text class="text__danger" v-if="error.date">{{"ေန႔စြဲ ထည့္ပါ"}}</v-ons-text>
            </label>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-minus-circle-outline" style="color: #FB8C00;" class="list-item__icon" v-if="form.supplier_id == ''"></v-ons-icon>
              <v-ons-icon icon="md-check" style="color: green;" class="list-item__icon" v-else></v-ons-icon>
            </div>
            <div class="center">
              <select style="width: 100%" v-model="form.supplier_id" class="form-control">
                <option value="" disabled selected><label>Supplier ေရြးပါ ...</label></option>
								<option v-for="supplier in option.suppliers" :value="supplier.id">
									{{supplier.name}}
								</option>
              </select>
              <v-ons-text class="text__danger" v-if="error.supplier_id">{{"Supplier ေရြးပါ"}}</v-ons-text>
            </div>
          </v-ons-list-item>
          <v-ons-list-item>
            <div class="left">
              <v-ons-icon icon="md-check" style="color: green;" class="list-item__icon" v-if="newItem.name != null"></v-ons-icon>
              <v-ons-icon icon="md-minus-circle-outline" style="color: #FB8C00;" class="list-item__icon" v-else></v-ons-icon>
            </div>
            <div class="center">
                <multiselect v-model="newItem" style="width:100%" placeholder="Select Item" label="name" track-by="id" :options="option.items" :option-height="104" :custom-label="ItemSelect" :show-labels="false">
                  <template slot="option" slot-scope="props">
                    <div class="row">
                      <div class="option__desc col-xs-8">
                        <strong class="option__title">{{ props.option.name }}</strong>
                      </div>
                    </div>
                  </template>
                </multiselect>

              <v-ons-text class="text__danger" v-if="error.item_id">{{"Item ေရြးပါ"}}</v-ons-text>
            </div>
          </v-ons-list-item>

          <v-ons-list-item v-show="newItem.name != null">
            <div class="left">
              <v-ons-icon icon="md-check" style="color: green;" class="list-item__icon" v-if="newQty != ''"></v-ons-icon>
              <v-ons-icon icon="md-minus-circle-outline" style="color: #FB8C00;" class="list-item__icon" v-else></v-ons-icon>
            </div>
            <label class="center">
              <v-ons-input type="number" float placeholder="QTY" v-model="newQty" style="width:80%">
              </v-ons-input>
            </label>
            <label class="right">
              <v-ons-text v-show="newQty != ''" @click.stop="addItem">
                <v-ons-icon icon="md-plus-circle" style="color:green" class="list-item__icon"> </v-ons-icon>
              </v-ons-text>
            </label>
          </v-ons-list-item>


          <div class="table-responsive">
              <table class="table table-bordered table-responsive">
                <thead>
                  <tr>
                    <th class="text-right">#</th>
                    <th class="text-center">SelectedItemName</th>
                    <th class="text-center">UnitPrice</th>
                    <th class="text-center">Qty</th>
                    <th class="text-center">Total</th>
                    <th class="text-center"></th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(itemin, index) in form.itemins" :key="itemin.id">
                    <td><v-ons-text>{{index + 1}}</v-ons-text></td>
                    <td><v-ons-text>{{itemin.name}}</v-ons-text></td>
                    <td>
                      <v-ons-input type="number" v-model="itemin.unit_price"></v-ons-input>
                    </td>
                    <td>
                      <v-ons-input type="number" v-model="itemin.qty"></v-ons-input>
                    </td>
                    <td class="text-right">
                      {{parseFloat(itemin.qty) * parseFloat(itemin.unit_price)}}
                    </td>
                    <td>
                      <v-ons-icon icon="md-close" style="color:red" class="list-item__icon" @click="form.itemins.splice(index, 1)"> </v-ons-icon>
                    </td>
                  </tr>
                </tbody>
                <tfoot>
                  <tr>
                    <td colspan="3"></td>
                    <td class="text-right"><v-ons-text>Sub Total</v-ons-text></td>
                    <td class="text-right">
                      <v-ons-text>{{subTotal}}
                      </v-ons-text>
                    </td>
                    <td></td>
                  </tr>
                  <tr>
                    <td colspan="3">
                    </td>
                    <td class="text-right"><v-ons-text>Remaining Balance</v-ons-text></td>
                    <td class="text-right">
                      <v-ons-text v-if="!remBalance">{{ form.rem_balance }} </v-ons-text>
                      <v-ons-input type="number" v-model="form.rem_balance" v-if="remBalance"></v-ons-input>
                    </td>
                    <td>
                      <v-ons-text @click="remBalance = !remBalance">
                        <v-ons-icon icon="md-edit" style="color:green" class="list-item__icon" v-if="!remBalance"> </v-ons-icon>

                        <v-ons-icon icon="md-check" style="color:green" class="list-item__icon"  v-if="remBalance"> </v-ons-icon>
                      </v-ons-text>
                    </td>
                  </tr>
                  <tr>
                    <td colspan="3">
                    </td>
                    <td class="text-right"><v-ons-text>Discount</v-ons-text></td>
                    <td class="text-right">
                      <v-ons-text v-if="!discount">{{ form.discount }} </v-ons-text>
                      <v-ons-input type="number" v-model="form.discount" v-if="discount"></v-ons-input>
                    </td>
                    <td>
                      <v-ons-text @click="discount = !discount">
                        <v-ons-icon icon="md-edit" style="color:green" class="list-item__icon" v-if="!discount"> </v-ons-icon>

                        <v-ons-icon icon="md-check" style="color:green" class="list-item__icon"  v-if="discount"> </v-ons-icon>
                      </v-ons-text>
                    </td>
                  </tr>
                  <tr>
                    <td colspan="3">

                    </td>
                    <td class="text-right"><v-ons-text>Total</v-ons-text></td>
                    <td class="text-right"><v-ons-text>{{total}}</v-ons-text></td>
                    <td></td>
                  </tr>
                </tfoot>
              </table>
            </div>
      </v-ons-list>
    </v-ons-card>
</v-ons-page>
</template>
<script>
import Vue from 'vue'
// import Flash from '../../../helpers/flash'
import { get, post, apiDomain, imgUrl } from '../../../helpers/api'
import Multiselect from 'vue-multiselect'
import ImageUpload from '../../../components/ImageUpload.vue'
import { toMulipartedForm } from '../../../helpers/form'
import StockInIndex from './Index.vue'

export default {
  components: { ImageUpload, Multiselect },
  // mixins: [ VueFocus.mixin ],
  data() {
    return {
      remBalance: false,
      discount: false,
      stockins: [],
      form: {
        itemQty: [],
        rembalance:'',
        itemins: []
      },
      years: [],
      selectYear: '',
      months: [],
      error: {},
      option: {
          suppliers: [],
          items: [],
          // years: [],
          // month: []
      },
      newItem: [{
        id:'', name:'', unit_price:'', qty:''
      }],
      newQty: '',
      isProcessing: false,
      initializeURL: apiDomain + `/stockins/create`,
      storeURL: apiDomain + `/stockins`,
      action: 'Create'
    }
  },
  created() {
    if(this.meta === 'edit') {
      this.initializeURL = apiDomain + `/stockins/${this.stockin.id}/edit`
      this.storeURL = apiDomain + `/stockins/${this.stockin.id}?_method=PUT`
      this.action = 'Update'
    }
    get(apiDomain + `/reports`)
      .then((res) => {
        this.years = res.data.years
      })
    get(this.initializeURL)
      .then((res) => {
        Vue.set(this.$data, 'form', res.data.form)
        if (this.meta === 'edit') {
          Vue.set(this.$data, 'selectYear', res.data.form.month.year_id)
          Vue.set(this.$data.form, 'itemQty', res.data.itemQty.itemins)
          Vue.set(this.$data.form, 'rembalance', res.data.itemQty.rem_balance)
        }
        Vue.set(this.$data, 'option', res.data.option)
      })
  },
  watch: {
      selectYear() {
        this.form.month_id = ''
        if (this.form.month_id.length == 0 ) {
          this.searchReportYear()
        }
        if (this.meta === 'edit') {
          this.form.month_id = this.form.month.id
        }
      }
      // selectMonth() {
      //   this.searchReportMonth()
      // }
  },
  computed: {
      subTotal() {
          return this.form.itemins.reduce(function(carry, itemin) {
              return carry + parseFloat(itemin.qty) * parseFloat(itemin.unit_price)
          }, 0)
      },
      total() {
          return this.subTotal - (parseFloat(this.form.discount)+parseFloat(this.form.rem_balance))
      }
  },
  methods: {
    searchReportYear: function() {
      get(apiDomain + `/reports/${this.selectYear}`)
        .then((res) => {
          this.months = res.data.months
        })
    },
    ItemSelect ({ name, code, brand_id, type_id, buy_price}) {
        return `${name}`
    },
    addItem() {
            this.form.itemins.push({
                item_id:  this.newItem.id,
                name:  this.newItem.name,
                description:  this.newItem.description,
                qty:  this.newQty,
                unit_price:  this.newItem.buy_price
            });
            this.newQty = ''
            this.newItem = ''

    },
    save() {
      // this.form.month_id = this.month.id
      const form = toMulipartedForm(this.form, this.meta)
      post(this.storeURL, form)
          .then((res) => {
              const apiUrl = apiDomain + `/stockins`
              if(res.data.saved) {
                this.$store.commit('navigator/replace', {
                  extends: StockInIndex,
                  data() {
                    return {
                      source: apiUrl,
                      animation: 'default'
                    }
                  }
                })
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
