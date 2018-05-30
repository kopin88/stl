<template>
  <div class="row">
    <div class="col-md-6 col-xs-12">
      <brand-index></brand-index>
    </div>
    <div class="col-md-6 col-xs-12">
      <type-index></type-index>
    </div>
  </div>
</template>
<script>
    import BrandIndex from './Brand.vue'
    import TypeIndex from './Type.vue'
    export default {
        name: 'BrandType',
        components: {
            BrandIndex, TypeIndex
        }
    }
</script>
