<template>
<v-ons-page>
    <custom-toolbar backLabel="Anim" :title="title">
      <template slot="right">
        <v-ons-icon style="color:white" icon="md-search" @click="searchItem"></v-ons-icon>
      </template>
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
    <item-viewer
      :source="source"
      :thead="thead"
      :filter="filter"
      :create="create"
      :title="title">
      <template slot-scope="props">
        <v-ons-list>
          <v-ons-list-item :action="onAction" :key="props.item.id" @click="transition(animation, props.item)" modifier="chevron" class="list-item-container">
            <div class="left">
              <img class="list-item__thumbnail" :src="imgLink + props.item.image" />
            </div>
            <div class="center">
              <span class="list-item__title">{{ props.item.name }}</span>
              <span class="list-item__subtitle">code : {{ props.item.code }}</span>
              <span class="list-item__subtitle">price : {{ props.item.sale_price }}</span>
            </div>
            <div class="right">
              {{props.item.qty_total}}
            </div>
          </v-ons-list-item>
        </v-ons-list>
      </template>
    </item-viewer>
  <v-ons-fab position="bottom right" @click="CreateItem(animation)">
    <v-ons-icon icon="md-plus"></v-ons-icon>
  </v-ons-fab>
</v-ons-page>
</template>


<script>
    import ItemShow from './Show.vue'
    import ItemSearch from './Search.vue'
    import ItemForm from './Form.vue'
    import ItemViewer from '../../../components/ItemViewer.vue'
    import {apiDomain, imgUrl} from '../../../helpers/api'

    export default {
        components: {ItemViewer},
        data() {
            return {
                state: 'initial',
                ratio: 0,
                animation: 'default',

                // items: ''
                title: 'Items List',
                imgLink: imgUrl + 'items/',
                source: apiDomain + '/items',
                create: apiDomain + '/items/create',
                thead: [
                    {title: 'Image', key: 'image', sort: false},
                    {title: 'Name', key: 'name', sort: true},
                    {title: 'Code', key: 'code', sort: true},
                    {title: 'Price', key: 'sale_price', sort: true},
                    {title: 'QTY', key: 'qty_total', sort: true}
                ],
                filter: [
                  'code', 'name', 'qty_total', 'buy_price', 'sale_price',
                  // Type
                  'type.name',
                  // Brand
                  'brand.name'

                ]
            }
        },
        methods: {
          onPull(ratio) {
            this.ratio = ratio;
          },
          onAction(done) {
            setTimeout(() => {
              this.items = [...this.items];
              done();
            }, 1500);
          },
          transition(name, data) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              item: data,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: ItemShow,
              data() {
                return {
                  animation: name,
                  item: data,
                  title: data.name,
                }
              }
            });
          },
          CreateItem(name) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: ItemForm,
              data() {
                return {
                  animation: name,
                  title: "Create Item",
                  meta: "create"
                }
              }
            });
          },
          searchItem(name) {
            this.$store.commit('navigator/options', {
              // Sets animations
              animation: name,
              // Resets default options
              callback: () => this.$store.commit('navigator/options', {})
            });

            this.$store.commit('navigator/push', {
              extends: ItemSearch,
              data() {
                return {
                  animation: name,
                  title: "",
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
