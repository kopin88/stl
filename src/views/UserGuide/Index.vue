<template>
  <v-ons-page>
	    <custom-toolbar backLabel="Anim" :title="'User Guide'">
        <!-- <template slot="right">
          <v-ons-icon style="color:white; padding-right:10px" icon="md-edit"  @click="editFinancialYear = true"></v-ons-icon>
        </template> -->
	    </custom-toolbar>

        <v-ons-list>
            <v-ons-list-header @click="viewItem">
              <v-ons-icon class="text__color list-icon" icon="md-chevron-down" size="20px" v-if="!isItem"></v-ons-icon>
              <v-ons-icon class="text__color list-icon" icon="md-chevron-up" size="20px" v-if="isItem"></v-ons-icon>
              <span>၁ - ပစၥည္းအမ်ိဳးအစား</span>
            </v-ons-list-header>
          <v-ons-list-item v-show="isItem">
            <!-- Tap to expand -->
            <div class="expandable-content">
              <p>ပစၥည္းအမ်ိဳးအစား စာရင္းျပဳစုပါ။</p>
              <p>အမည္(Name)၊ သတ္မွတ္ခ်က္(Code) ေသခ်ာ သတ္မွတ္ထည့္ပါ။ <br>Search မွ ျပန္ရွာလွၽင္ အသံုးျပဳႏိုင္ပါသည္။</p>
              <p>Brand & Type စနစ္တက်ထည့္သြင္းပါ။</p>
              <p>ပံုထည့္ပါ။ <br>(ပံုမရွိပါက မိမိ Logo ေသာ္လည္းေကာင္း မျဖစ္မေန တစ္ခုထည့္ပါ)။</p>
              <p>အျခားအခ်က္အလက္မ်ား မရွိပါက '-' ထားခဲ့ပါ။</p>
            </div>
          </v-ons-list-item>
        </v-ons-list>
        <v-ons-list>
          <!-- <v-ons-list-item> -->
            <v-ons-list-header @click="viewSupplier">
              <v-ons-icon class="text__color list-icon" icon="md-chevron-down" size="20px" v-if="!isSupplier"></v-ons-icon>
              <v-ons-icon class="text__color list-icon" icon="md-chevron-up" size="20px" v-if="isSupplier"></v-ons-icon>
              <span>၂ - မိမိ၀ယ္ယူသူ (သို႔) ကုန္သြင္းသူ (Supplier)</span>
            </v-ons-list-header>
          <!-- </v-ons-list-item> -->
          <v-ons-list-item v-show="isSupplier">
            <!-- Tap to expand -->
            <div class="expandable-content">
              <p>မိမိ ၀ယ္ယူေနက် ဆိုင္၊ ကုမၸဏီ ထည့္ရန္။</p>
              <p>သီးျခားမရွိပါက အနည္းဆံုး General အျဖစ္ တစ္ခု ထည့္ပါ။</p>
              <p>အမည္(Name)၊ သတ္မွတ္ခ်က္(Code) ေသခ်ာ သတ္မွတ္ထည့္ပါ။ <br>Search မွ ျပန္ရွာလွၽင္ အသံုးျပဳႏိုင္ပါသည္။</p>
              <p>ပံုထည့္ပါ။ <br>(ပံုမရွိပါက မိမိ Logo ေသာ္လည္းေကာင္း မျဖစ္မေန တစ္ခုထည့္ပါ)။</p>
              <p>အျခားအခ်က္အလက္မ်ား မရွိပါက '-' ထားခဲ့ပါ။</p>
            </div>
          </v-ons-list-item>
        </v-ons-list>
        <v-ons-list>
            <v-ons-list-header @click="viewCustomer">
              <v-ons-icon class="text__color list-icon" icon="md-chevron-down" size="20px" v-if="!isCustomer"></v-ons-icon>
              <v-ons-icon class="text__color list-icon" icon="md-chevron-up" size="20px" v-if="isCustomer"></v-ons-icon>
              <span>၃ - မိမိထံမွ၀ယ္ယူသူ (သို႔) ေဖာက္သည္ (Customer)</span>
            </v-ons-list-header>
          <v-ons-list-item v-show="isCustomer">
            <!-- Tap to expand -->
            <div class="expandable-content">
              <p>မိမိထံမွ ၀ယ္ယူေနက် ေဖာက္သည္(customer) ထည့္ရန္။</p>
              <p>သီးျခားမရွိပါက အနည္းဆံုး General အျဖစ္ တစ္ခု ထည့္ပါ။</p>
              <p>အမည္(Name)၊ သတ္မွတ္ခ်က္(Code) ေသခ်ာ သတ္မွတ္ထည့္ပါ။ <br>Search မွ ျပန္ရွာလွၽင္ အသံုးျပဳႏိုင္ပါသည္။</p>
              <p>ပံုထည့္ပါ။ <br>(ပံုမရွိပါက မိမိ Logo ေသာ္လည္းေကာင္း မျဖစ္မေန တစ္ခုထည့္ပါ)။</p>
              <p>အျခားအခ်က္အလက္မ်ား မရွိပါက '-' ထားခဲ့ပါ။</p>
            </div>
          </v-ons-list-item>
        </v-ons-list>
        <v-ons-list>
            <v-ons-list-header @click="viewAccount">
              <v-ons-icon class="text__color list-icon" icon="md-chevron-down" size="20px" v-if="!isAccount"></v-ons-icon>
              <v-ons-icon class="text__color list-icon" icon="md-chevron-up" size="20px" v-if="isAccount"></v-ons-icon>
              <span>၄ - ေငြစာရင္း (Account)</span>
            </v-ons-list-header>
          <v-ons-list-item v-show="isAccount">
            <!-- Tap to expand -->
            <div class="expandable-content">
              <p><v-ons-icon icon="md-plus-circle" class="text__color"></v-ons-icon> ႏွိပ္ၿပီး ဘ႑ာေရးႏွစ္ (သို႔) ခုနွစ္ ထည့္ပါ။</p>
              <hr>
              <p><v-ons-icon icon="md-plus-circle" class="text__color"></v-ons-icon> ႏွိပ္ၿပီး လထည့္ရန္ </p>
              <p>လေရြးပါ၊ ေခါင္းစဥ္ထည့္ပါ။</p>
              <p>Opening Balance မွာ စာရင္းဖြင့္ ထည့္ပါ (သို႔) ယခင္ႏွစ္လက္က်န္ ထည့္ပါ (သို႔) ေရွ႕လေရြးေပးပါ။</p>
              <hr>
              <p>အ၀င္ ထည့္ရန္ <b>Income</b> ကို ေရြးၿပီး <v-ons-icon icon="md-plus-circle" class="text__color"></v-ons-icon> ႏွိပ္ပါ။ </p>
              <p>Name ထည့္ပါ။</p>
              <p>ေငြစာရင္းေခါင္းစဥ္ေရြးပါ (သုိ႔) <v-ons-icon icon="md-plus-square" class="text__color"></v-ons-icon> ႏွိပ္ၿပီး အသစ္ထည့္ပါ။</p>
              <p>Amount မွာ Number(Only) ထည့္ပါ။</p>
              <p>ေဘာင္ခ်ာ ရွိပါက/ထည့္လိုက ထည့္ႏိုင္သည္။</p>
              <hr>
              <p>အထြက္ ထည့္ရန္ <b>Expence</b> ကို ေရြးၿပီး <v-ons-icon icon="md-plus-circle" class="text__color"></v-ons-icon> ႏွိပ္ပါ။ </p>
              <p>Name ထည့္ပါ။</p>
              <p>ေငြစာရင္းေခါင္းစဥ္ေရြးပါ (သုိ႔) <v-ons-icon icon="md-plus-square" class="text__color"></v-ons-icon> ႏွိပ္ၿပီး အသစ္ထည့္ပါ။</p>
              <p>Amount မွာ Number(Only) ထည့္ပါ။</p>
              <p>ေဘာင္ခ်ာ ရွိပါက/ထည့္လိုက ထည့္ႏိုင္သည္။</p>
              <hr>
              <p><b>Report</b> တြင္ 'Total incomes', 'Total expenses', 'Total Balance' မ်ား စာရင္း မညီပါက <v-ons-icon class="text-warning" icon="md-alert-circle"></v-ons-icon> ကိုေတြ႕ရမည္ျဖစ္ၿပီး၊ တစ္ခ်က္ႏွိပ္လိုက္ပါက <v-ons-icon class="text__color" icon="md-check-circle"></v-ons-icon> ကိုေတြ႕ရမည္ျဖစ္ၿပီး စာရင္း ညီသြားပါလိမ့္မည္။</p>
            </div>
          </v-ons-list-item>
        </v-ons-list>
        <v-ons-list>
            <v-ons-list-header @click="viewStockIn">
              <v-ons-icon class="text__color list-icon" icon="md-chevron-down" size="20px" v-if="!isStockIn"></v-ons-icon>
              <v-ons-icon class="text__color list-icon" icon="md-chevron-up" size="20px" v-if="isStockIn"></v-ons-icon>
              <span>၅ - ပစၥည္း အ၀င္ (Stock In)</span>
            </v-ons-list-header>
          <v-ons-list-item v-show="isStockIn">
            <!-- Tap to expand -->
            <div class="expandable-content">
            </div>
          </v-ons-list-item>
        </v-ons-list>
        <v-ons-list>
            <v-ons-list-header @click="viewStockOut">
              <v-ons-icon class="text__color list-icon" icon="md-chevron-down" size="20px" v-if="!isStockOut"></v-ons-icon>
              <v-ons-icon class="text__color list-icon" icon="md-chevron-up" size="20px" v-if="isStockOut"></v-ons-icon>
              <span>၆ - ပစၥည္း အထြက္ (Stock Out)</span>
            </v-ons-list-header>
          <v-ons-list-item v-show="isStockOut">
            <!-- Tap to expand -->
            <div class="expandable-content">
            </div>
          </v-ons-list-item>
        </v-ons-list>
  </v-ons-page>
</template>


<script>
export default{
  data() {
    return {
      isItem: false,
      isSupplier: false,
      isCustomer: false,
      isAccount: false,
      isStockIn: false,
      isStockOut: false
    }
  },
  methods: {
    viewItem() {
      this.isItem = !this.isItem
      this.isSupplier = false
      this.isCustomer = false
      this.isAccount = false
      this.isStockIn = false
      this.isStockOut = false
    },
    viewSupplier() {
      this.isSupplier = !this.isSupplier
      this.isItem = false
      this.isCustomer = false
      this.isAccount = false
      this.isStockIn = false
      this.isStockOut = false
    },
    viewCustomer() {
      this.isCustomer = !this.isCustomer
      this.isItem = false
      this.isSupplier = false
      this.isAccount = false
      this.isStockIn = false
      this.isStockOut = false
    },
    viewAccount() {
      this.isAccount = !this.isAccount
      this.isItem = false
      this.isSupplier = false
      this.isCustomer = false
      this.isStockIn = false
      this.isStockOut = false
    },
    viewStockIn() {
      this.isStockIn = !this.isStockIn
      this.isItem = false
      this.isSupplier = false
      this.isCustomer = false
      this.isAccount = false
      this.isStockOut = false
    },
    viewStockOut() {
      this.isStockOut = !this.isStockOut
      this.isItem = false
      this.isSupplier = false
      this.isCustomer = false
      this.isAccount = false
      this.isStockIn = false
    },
  }
}
</script>

<style scoped>
.button-margin {
  margin: 6px 0;
}
.list-icon {
  vertical-align: middle;
  padding: 0 10px;
}
</style>
