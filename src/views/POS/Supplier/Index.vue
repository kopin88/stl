<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="title">
    </custom-toolbar>

    <v-ons-pull-hook
      :action="onAction"
      :fixed-content="md"
      :height="md ? 84 : 64"
      :on-pull="md && onPull || null"
      @changestate="state = $event.state"
    >

      <!-- Show this on iOS -->
      <v-ons-icon v-if="!md"
        size="22px"
        class="pull-hook-spinner"
        :icon="state === 'action' ? 'fa-spinner' : 'fa-arrow-down'"
        :rotate="state === 'preaction' && 180"
        :spin="state === 'action'"
      ></v-ons-icon>

      <!-- Show this on Material Design -->
      <div v-else class="pull-hook-progress">
        <v-ons-progress-circular
          :value="ratio * 100"
          :indeterminate="state === 'action'"
          :style="{ transform: `rotate(${ratio}turn)` }"
        ></v-ons-progress-circular>
      </div>

    </v-ons-pull-hook>

  <v-ons-list>
    <v-ons-list-item v-for="supplier in suppliers" :key="supplier.id" @click="transition(animation, supplier)" modifier="chevron" class="list-item-container">
      <div class="left">
        <img class="list-item__thumbnail" :src="imgLink + supplier.image">
      </div>
      <div class="center">
        <span class="list-item__title">{{ supplier.name }}</span><span class="list-item__subtitle">{{ supplier.code }}</span>
      </div>
      <div class="right">
        {{supplier.qty_total}}
      </div>
    </v-ons-list-item>
  </v-ons-list>
  <v-ons-fab position="bottom right" @click="CreateSupplier(animation)">
    <v-ons-icon icon="md-plus"></v-ons-icon>
  </v-ons-fab>
</v-ons-page>
</template>


<script>
    import SupplierShow from './Show.vue'
    import SupplierForm from './Form.vue'
    import {apiDomain, imgUrl} from '../../../helpers/api'

    export default {
        data() {
            return {
                state: 'initial',
                ratio: 0,
                animation: 'default',
                imgLink: imgUrl + 'suppliers/',
            }
        },
        methods: {
          onPull(ratio) {
            this.ratio = ratio;
          },
          onAction(done) {
            setTimeout(() => {
              this.suppliers = [...this.suppliers];
              done();
            }, 1500);
          },
          transition(name, data) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              supplier: data,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: SupplierShow,
              data() {
                return {
                  animation: name,
                  supplier: data,
                  title: data.name,
                }
              }
            });
          },
          CreateSupplier(name) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: SupplierForm,
              data() {
                return {
                  animation: name,
                  title: "Create Supplier",
                }
              }
            });
          }
        }
    }
</script>

<style>
.pull-hook-spinner {
  color: #666;
  transition: transform .25s ease-in-out;
}

.pull-hook-progress {
  background-color: white;
  width: 32px;
  height: 32px;
  margin: 30px auto 0;
  border-radius: 100%;
  position: relative;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
  display: inline-block;
  line-height: 0px;
}

.pull-hook-progress .progress-circular {
  width: 24px;
  height: 24px;
  position: absolute;
  top: 4px;
  left: 4px;
}

.pull-hook-progress .progress-circular__primary {
  transition: stroke-dashoffset 0s;
}
</style>
