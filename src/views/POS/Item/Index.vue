<template>
  <v-ons-page>
    <page-toolbar :backLink="'/'" :title="title"></page-toolbar>
    <v-ons-card style="margin-top:63px; padding-bottom:80px">
      <data-viewer :source="source" :thead="thead" :filter="filter" :create="create" :title="title">
        <template slot-scope="props">
          <v-ons-list>
            <v-ons-list-item @click="$router.push('/items/' + props.item.id)" modifier="chevron" class="list-item-container">
              <div class="left">
                <img class="list-item__thumbnail" :src="imgLink + props.item.image">
              </div>
              <div class="center">
                <span class="list-item__title">{{ props.item.name }}</span><span class="list-item__subtitle">{{ props.item.code }}</span>
              </div>
              <div class="right">
                {{props.item.qty_total}}
              </div>
          </v-ons-list-item>
        </v-ons-list>
        </template>
      </data-viewer>
    </v-ons-card>
    <v-ons-fab position="bottom right" @click="$router.push('/items/create')">
      <v-ons-icon icon="md-plus"></v-ons-icon>
    </v-ons-fab>
  </v-ons-page>
</template>

<script>
    import DataViewer from '../../../components/DataViewer.vue'
    import PageToolbar from '../../Layout/Toolbar.vue'
    import {apiDomain, imgUrl} from '../../../helpers/api'

    export default {
        name: 'ItemIndex',
        data() {
            return {
                title: 'Items List',
                imgLink: imgUrl + 'items/',
                source: apiDomain + '/items',
                create: apiDomain + '/items/create',
                thead: [
                    // {title: 'Image', key: 'image', sort: false},
                    // {title: 'Code', key: 'code', sort: true},
                    {title: 'Items', key: 'code', sort: true},
                    // {title: 'Price', key: 'sale_price', sort: true},
                    // {title: 'QTY', key: 'qty_total', sort: true}
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
        components: {
            DataViewer,
            PageToolbar
        }
    }
</script>


<style scoped>
.img {
  height: 80px;
  width: 80px;
  border-radius: 10%;
}
</style>
