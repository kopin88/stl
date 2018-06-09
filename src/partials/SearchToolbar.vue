<template>
  <v-ons-toolbar>
    <div class="left">
      <slot name="left">
        <v-ons-back-button v-if="backLabel">
          {{ backLabel }}
        </v-ons-back-button>
      </slot>
    </div>
    <div class="center"><slot><slot name="center"></slot></slot></div>
    <div class="right"><slot name="right"></slot></div>
  </v-ons-toolbar>
</template>

<script>
export default {
  props: ['title', 'backLabel']
};
</script>

<style>
  .back-button--material__icon {
    fill:#343a40;
    /* fill:#28a745; */
  }
</style>
