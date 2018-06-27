<template>
  <v-ons-page modifier="white">
    <div class="profile-pic">
      <img src="../../assets/stl-logo.png" style="padding:30px">
    </div>

    <v-ons-list-title>Menu</v-ons-list-title>
    <v-ons-list>
      <v-ons-list-item
        :modifier="md && 'nodivider'"
        @click="$router.push('years')"
      >
        <div class="left">
          <v-ons-icon fixed-width class="list-item__icon" icon="md-edit"></v-ons-icon>
        </div>
        <div class="center">
          ေငြစာရင္း
        </div>
        <div class="right">
          <v-ons-icon icon="fa-external-link"></v-ons-icon>
        </div>
      </v-ons-list-item>
      <v-ons-list-item
        :modifier="md && 'nodivider'"
        @click="$router.push('suppliers')"
      >
        <div class="left">
          <v-ons-icon fixed-width class="list-item__icon" icon="md-face"></v-ons-icon>
        </div>
        <div class="center">
          Supplier
        </div>
        <div class="right">
          <v-ons-icon icon="fa-external-link"></v-ons-icon>
        </div>
      </v-ons-list-item>
    </v-ons-list>
  </v-ons-page>
</template>

<script>
export default {
};
</script>

<style scoped>
.profile-pic {
  width: 100%;
  background-color: #fff;
  border-bottom: 1px solid #DDD;
  color: rgba(0, 0, 0, .56);
  padding-bottom: 8px;
}
.page--material .profile-pic {
  background-color: #f6f6f6;
}

.profile-pic > img {
  display: block;
  max-width: 100%;
}
</style>

<style>
.page--material__background.page--white__background {
  background-color: #fff;
}
</style>
