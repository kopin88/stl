<template>
  <v-ons-toolbar class="toolbar__color">
    <div class="left">
      <slot name="left">
        <v-ons-back-button v-if="backLabel">
          {{ backLabel }}
        </v-ons-back-button>
      </slot>
    </div>
    <div class="center" style="color:white"><slot>{{ title }}</slot></div>
    <div class="right" style="text-align:center"><slot name="right"></slot></div>
  </v-ons-toolbar>
</template>

<script>
export default {
  props: ['title', 'backLabel']
};
</script>

<style>
  .back-button--material__icon {
    fill:#ffffff;
  }
</style>
