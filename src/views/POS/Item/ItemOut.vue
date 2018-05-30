<template>
    <item-out-data-viewer :source="source" :thead="thead" :filter="filter">
        <template slot-scope="props">
            <tr v-if="props.item.stock_out.active == 0">
              <td class="text-center">{{props.item.stock_out.date | moment("DD - MMM - YYYY") }}</td>
							<td class="text-center">{{props.item.stock_out.invoice_no}}</td>
							<td class="text-right">{{props.item.unit_price}}</td>
							<td class="text-right">{{props.item.qty}}</td>
							<td class="text-right">{{props.item.qty * props.item.unit_price}}</td>
						</tr>
        </template>
    </item-out-data-viewer>
</template>
<script>
    import ItemOutDataViewer from '../../../components/ItemOutDataViewer.vue'

    export default {
        name: 'ItemOut',
        data() {
            return {
                source: `/api/items/${this.$route.params.id}`,
                thead: [
                    {title: 'Date', key: 'stock_out.date', sort: true},
                    {title: 'Vouncher No', key: 'stock_out.invoice_no', sort: true},
                    {title: 'Unit Price', key: 'unit_price', sort: false},
                    {title: 'QTY', key: 'qty', sort: false},
                    {title: 'Total', key: 'qty*unit_price', sort: false}
                ],
                filter: [
                  'stock_out.date', 'stock_out.invoice_no',

                ]
            }
        },
        components: {
            ItemOutDataViewer
        }
    }
</script>
