<template>
  <v-ons-page  v-if="authState.api_token" v-cloak>
    <v-ons-splitter>
      <v-ons-splitter-side swipeable side="left" collapse=""
        width="260px"
        :swipe-target-width="md && 25"
        :animation="md ? 'overlay' : 'reveal'"
        :open.sync="isOpen"
      >
        <menu-page></menu-page>
      </v-ons-splitter-side>

      <v-ons-splitter-content>
        <app-tabbar></app-tabbar>
      </v-ons-splitter-content>
    </v-ons-splitter>
  </v-ons-page>
  <v-ons-page  v-else>
    <v-ons-splitter>
      <v-ons-splitter-content>
        <log-in></log-in>
      </v-ons-splitter-content>
    </v-ons-splitter>
  </v-ons-page>
</template>

<script>
import AppTabbar from './AppTabbar.vue';
import MenuPage from './pages/Menu.vue';
import LogIn from './views/Auth/Login';
import Auth from './store/auth';
import { post, interceptors } from './helpers/api';

export default {
  data() {
    return {
      authState: Auth.state,
      // flash: Flash.state
    }
  },
  created() {
    // global error http handler
    interceptors((err) => {
    })
    Auth.initialize()
  },
  computed: {
    isOpen: {
      get() {
        return this.$store.state.splitter.open;
      },
      set(newValue) {
        this.$store.commit('splitter/toggle', newValue)
      }
    }
  },
  methods: {
    logout() {
      post('/api/logout')
          .then((res) => {
              if(res.data.done) {
                  // remove token
                  Auth.remove()
                  // Flash.setSuccess('You have successfully logged out.')
                  // this.$router.back()
              }
          })
    }
  },
  components: { AppTabbar, MenuPage, LogIn }
};
</script>

<style>
ons-splitter-side[animation=overlay] {
  border-left: 1px solid #bbb;
}
</style>
