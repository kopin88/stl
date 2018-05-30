<template>
      <brand-type-data-viewer :source="source" :thead="thead" :filter="filter" :title="title">
        <template slot-scope="props">
          <tr>
            <td>{{props.item.name}}</td>
            <td>{{props.item.descriptions}}</td>
          </tr>
        </template>
      </brand-type-data-viewer>
</template>
<script>
    import Vue from 'vue'
    import BrandTypeDataViewer from '../../../components/BrandTypeDataViewer.vue'
    import { get, post } from '../../../helpers/api'
  	import { toMulipartedForm } from '../../../helpers/form'

    export default {
        name: 'TypeIndex',
        data() {
            return {
                title: 'Types',
                source: '/api/types',
                thead: [
                    {title: 'Name', key: 'name', sort: true},
                    {title: 'Description', key: 'descriptions', sort: true}
                ],
                filter: [
                  'name','descriptions'
                ]
            }
        },
        components: {
            BrandTypeDataViewer
        }
    }
</script>
