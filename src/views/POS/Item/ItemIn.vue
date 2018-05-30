<template>
    <item-in-data-viewer :source="source" :thead="thead" :filter="filter">
        <template slot-scope="props">
            <tr v-if="props.item.stock_in.active == 0">
              <td class="text-center">{{props.item.stock_in.date | moment("DD - MMM - YYYY") }}</td>
							<td class="text-center">{{props.item.stock_in.invoice_no}}</td>
							<td class="text-right">{{props.item.unit_price}}</td>
							<td class="text-right">{{props.item.qty}}</td>
							<td class="text-right">{{props.item.qty * props.item.unit_price}}</td>
						</tr>
        </template>
    </item-in-data-viewer>
</template>
<script>
    import ItemInDataViewer from '../../../components/ItemInDataViewer.vue'

    export default {
        name: 'ItemIn',
        data() {
            return {
                source: `/api/items/${this.$route.params.id}`,
                thead: [
                    {title: 'Date', key: 'stock_in.date', sort: true},
                    {title: 'Vouncher No', key: 'stock_in.invoice_no', sort: true},
                    {title: 'Unit Price', key: 'unit_price', sort: false},
                    {title: 'QTY', key: 'qty', sort: false},
                    {title: 'Total', key: 'qty*unit_price', sort: false}
                ],
                filter: [
                  'stock_in.date', 'stock_in.invoice_no',

                ]
            }
        },
        components: {
            ItemInDataViewer
        }
    }
</script>
