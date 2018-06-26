<template>
  <v-ons-page>
    <v-ons-dialog cancelable
      class="lorem-dialog"
      :visible.sync="dialogVisible"
    >
      <v-ons-page>
        <v-ons-toolbar>
          <div class="left">
            <v-ons-icon class="text__color" style="padding-left:10px" @click="cancel(animation)" icon="md-long-arrow-left"></v-ons-icon>
          </div>
          <div class="center">Create Financial Year</div>
          <div class="right">
            <v-ons-icon class="text__color" style="padding-right:10px" icon="md-check" @click="saveFinancialYear(animation)"></v-ons-icon>
          </div>
        </v-ons-toolbar>
        <v-ons-list>
            <v-ons-list-item style="padding:30px 15px">
              <v-ons-input float placeholder="Financial Year Name" style="width:100%;font-family:Zawgyi-One;" v-model="year.name">
              </v-ons-input>
            </v-ons-list-item>
          </v-ons-list>
      </v-ons-page>
    </v-ons-dialog>
  </v-ons-page>
</template>
<script>
import { get, post, apiDomain } from '../../../helpers/api'
import { toMulipartedForm } from '../../../helpers/form'
export default {
  data(){
    return {
      dialogVisible: true,
      animation: 'default',
      createFinancialYear: true,
      isProcessingYear: false,
      meta: 'create',
      yearURL: apiDomain + '/years',
      year:{name:''},
    }
  },
  methods: {
    saveFinancialYear() {
      const year = toMulipartedForm(this.year, this.meta)
      post(this.yearURL, year)
        .then((res) => {
          if (res.data.saved) {
            this.$store.commit('navigator/pop')
          }
        })
    },
    cancel(name) {
      this.$store.commit('navigator/pop')
    }

  }
}
</script>

<style>
.lorem-dialog .dialog-container {
  height: 200px;
}
</style>
